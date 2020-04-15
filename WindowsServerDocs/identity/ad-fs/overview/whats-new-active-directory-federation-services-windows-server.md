---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Windows Server 2016 的 Active Directory 同盟服務新功能
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/22/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e88297bdbd55d2f834f1bff72b6d05bdf356bb85
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860251"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Active Directory 同盟服務的新功能


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>Windows Server 2019 的 Active Directory 同盟服務中的新功能

### <a name="protected-logins"></a>受保護的登入
以下是將簡短說明 AD FS 2019 中提供的受保護登入：
- **以外部驗證提供者作為主要因素** - 客戶現在可以使用第三方驗證產品作為第一個因素，而不將密碼公開為第一個因素。 在外部驗證提供者可以證明雙因素的情況下，即可宣告 MFA。 
- **以密碼驗證作為額外的驗證** - 客戶具有受到完整支援的預設選項，可在使用無密碼的選項作為第一個因素之後，才使用密碼作為額外的因素。 這可改善客戶使用 ADFS 2016 的體驗；過去，客戶必須下載依現狀支援的 github 配接器。 
- **可插式風險評估模組** - 客戶現在可以建立自己的外掛程式模組，以在預先驗證階段封鎖特定類型的要求。 這可讓客戶更輕鬆地使用身分識別保護之類的雲端智慧來封鎖風險性使用者的登入或有風險的交易。  如需詳細資訊，請參閱[使用 AD FS 2019 風險評估模型建置外掛程式](../../ad-fs/development/ad-fs-risk-assessment-model.md) 
- **ESL 改進** - 藉由新增下列功能改善2016 中的 ESL QFE
    - 可讓客戶處於稽核模式，同時受到 ADFS 2012R2 之後提供的「傳統」外部網路鎖定功能的保護。 目前的 2016 客戶在稽核模式下不受保護。 
    - 針對熟悉的位置啟用獨立的鎖定閾值。 如此，使用共同服務帳戶執行的多個應用程式執行個體，就能在最少的影響下變換密碼。 

### <a name="additional-security-improvements"></a>其他安全性改進
AD FS 2019 提供下列額外的安全性改進：
- **使用智慧卡登入的遠端 PSH** - 客戶現在可以使用智慧卡透過 PSH 從遠端連線至 ADFS，然後使用該服務來管理所有 PSH 功能，包括多節點 PSH Cmdlet。
- **HTTP 標頭自訂** - 客戶現在可以自訂在 ADFS 回應期間發出的 HTTP 標頭。 其中包括下列標頭
     - HSTS：此標頭會傳達只能在 HTTPS 端點上使用以強制執行相容瀏覽器的 ADFS 端點
     - x-frame-options：可讓 ADFS 系統管理員允許特定的信賴憑證者內嵌 ADFS 互動式登入頁面的 iFrame。 此標頭應謹慎使用，且限用於 HTTPS 主機上。 
     - 未來的標頭：您也可以設定其他未來的標頭。 

如需詳細資訊，請參閱[使用 AD FS 2019 自訂 HTTP 安全性回應標頭](../../ad-fs/operations/customize-http-security-headers-ad-fs.md) 

### <a name="authenticationpolicy-capabilities"></a>驗證/原則功能
以下是 AD FS 2019 中的驗證/原則功能：
- **就個別的 RP 指定額外驗證的驗證方法** - 客戶現在可以使用宣告規則，決定要叫用哪個額外的驗證提供者作為額外的驗證提供者。 這可用在 2 個使用案例中
    - 客戶要從一個額外的驗證提供者轉換到另一個。 如此，當他們將使用者上架至較新的驗證提供者時，將可使用群組來控制要呼叫哪個額外的驗證提供者。
    - 針對特定應用程式，客戶需要特定的額外驗證提供者 (例如憑證)。 
- **將 TLS 型裝置驗證限定於有需要的應用程式** - 客戶現在可以將用戶端 TLS 型裝置驗證限定於執行裝置條件式存取的應用程式。 這樣可以避免對不需要 TLS 型裝置驗證的應用程式發出任何不必要的裝置驗證提示 (或避免在用戶端應用程式無法處理時發生失敗的狀況)。
- **MFA 有效期限支援** - AD FS 現在支援根據第二因素認證的有效期限重新執行第二因素認證的功能。 這可讓客戶先透過 2 個因素執行初始交易，而其後僅定期發出第二個因素的提示。 此設定僅適用於可在要求中提供額外參數的應用程式，且在 ADFS 中並非可設定的設定。 在已設定 [記住我的 MFA X 天]，並且在 Azure AD 中的同盟網域信任設定上將 'supportsMFA' 旗標設為 true 時，Azure AD 才會支援此參數。 

### <a name="sign-in-sso-improvements"></a>登入 SSO 的改進
AD FS 2019 做了下列登入 SSO 改進：

- [具有置中主題的分頁 UX](../operations/AD-FS-paginated-sign-in.md) - ADFS 現已移至分頁 UX 流程，讓 ADFS 能夠進行驗證，並提供更流暢的登入體驗。 ADFS 現在會使用置中的 UI (而非位於畫面右側)。 您可能需要較新的標誌和背景影像，才能與此體驗協調。 這也會反映 Azure AD 中提供的功能。
- **Bug 修正：Win10 裝置在執行 PRT 驗證時具有持續性的 SSO 狀態**    此修正解決了對 Windows 10 裝置使用 PRT 驗證時未持續保存 MFA 狀態的問題。 此問題會導致使用者經常收到第二因素認證 (MFA) 的提示。 此修正也使透過用戶端 TLS 和透過 PRT 機制順利執行的裝置驗證能有一致的體驗。 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>建置新式企業營運應用程式的支援
AD FS 2019 新增了下列建置新式 LOB 應用程式的支援：

 - **Oauth 裝置流程/設定檔** - AD FS 現已支援 OAuth 裝置流程設定檔，以在沒有 UI 介面區域可支援豐富登入體驗的裝置上執行登入。 這可讓使用者在不同的裝置上完成登入體驗。 這是 Azure Stack 中的 Azure CLI 體驗所需的功能，而且也可在其他情況下使用。 
 - **移除「資源」參數** - AD FS 現已不再需要指定內嵌於目前 Oauth 規格中的資源參數。 用戶端現在除了要求的權限以外，也可提供信賴憑證者信任識別碼作為範圍參數。 
 - **AD FS 回應中的 CORS 標頭** - 客戶現在可以建置單頁應用程式，以允許用戶端 JS 程式庫從 AD FS 的 OIDC 探索文件中查詢簽署金鑰，藉以驗證 id_token 的簽章。 
 - **PKCE 支援** - AD FS 新增了 PKCE 支援，以在 OAuth 中提供安全的驗證碼流程。 這樣可以提高此流程的安全性層級，以避免程式碼遭到不同用戶端的攔截並加以重新執行。 
 - **Bug 修正：傳送 x5t 和 kid 宣告** - 這是次要的 Bug 修正。 AD FS 現在會額外傳送 'kid' 宣告，以提供用來驗證簽章的金鑰識別碼提示。 過去，AD FS 為此只會傳送 'x5t' 宣告。

### <a name="supportability-improvements"></a>支援能力的改進
下列可支援能力改進並不屬於 AD FS 2019：
- **將錯誤詳細資料傳送給 AD FS 系統管理員** - 可讓系統管理員設定使用者，以傳送其與使用者驗證失敗有關、且要儲存為壓縮欄位以便取用的偵錯記錄。 系統管理員也可設定 SMTP 連線，以將壓縮檔案自動傳送至分級電子郵件帳戶，或根據電子郵件自動建立票證。 

### <a name="deployment-updates"></a>部署更新
AD FS 2019 現在包含下列部署更新：
- **伺服器陣列行為層級 2019** - 如同 AD FS 2016，若要啟用上述新功能，必須要有新的伺服器陣列行為層級版本。 其可行的途徑如下：
    - 2012 R2-> 2019
    - 2016 -> 2019     

### <a name="saml-updates"></a>SAML 更新
以下是 AD FS 2019 中的 SAML 更新：
- **Bug 修正：修正彙總同盟中的 Bug** - 在彙總同盟支援 (例如 InCommon) 方面有許多 Bug 修正。 這些修正與下列層面有關： 
  - 改善了在彙總同盟中繼資料文件中調整大量實體的效能。過去，此作業會失敗並出現 "ADMIN0017" 錯誤。 
  - 透過 Get-AdfsRelyingPartyTrustsGroup PSH Cmdlet 使用 'ScopeGroupID' 參數進行查詢。 
  - 處理關於 entityID 重複的錯誤狀況


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>範圍參數中的 Azure AD 樣式資源規格 
過去，無論在任何驗證要求中，AD FS 都需要將所需的資源和範圍放在個別的參數中。 例如，一般的 oauth 要求會顯示如下：7 **https:&#47;&#47;fs.contoso.com/adfs/oauth2/authorize?</br>response_type=code&client_id=claimsxrayclient&resource=urn:microsoft:</br>adfs:claimsxray&scope=oauth&redirect_uri=https:&#47;&#47;adfshelp.microsoft.com/</br> ClaimsXray/TokenResponse&prompt=login**
 
使用 Server 2019 上的 AD FS，您現在可以傳遞範圍參數中內嵌的資源值。 此做法也與您對 Azure AD 執行驗證的方式相一致。 

範圍參數現在可以組織成以空格分隔的清單，其中的每個項目都會建構為資源/範圍。 

> [!NOTE]
> 在驗證要求中只能指定一個資源。 如果要求中包含多個資源，AD FS 將會傳回錯誤，且驗證將會失敗。 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>OAuth 的 Proof Key for Code Exchange (PKCE) 支援 
使用授權碼授與的 OAuth 公用用戶端很容易遭受授權碼攔截攻擊。  這項攻擊在 RFC 7636 中有詳細說明。 為了緩解此攻擊，Server 2019 中的 AD FS 支援 OAuth 授權碼授與流程的 Proof Key for Code Exchange (PKCE)。 
 
若要運用 PKCE 支援，此規格對 OAuth 2.0 授權和存取權杖要求新增了額外的參數。

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. 用戶端建立並記錄名為 "code_verifier" 的密碼，並衍生轉換後的版本 "t(code_verifier)" (名為 "code_challenge")，而該版本會在 OAuth 2.0 授權要求中連同轉換方法 "t_m" 一起傳送。 

B. 授權端點如常回應，但記錄了 "t(code_verifier)" 和轉換方法。 

C. 接著，用戶端如常傳送存取權杖要求中的授權碼，但其中包含在 (A) 產生的 "code_verifier" 秘密。 

D. AD FS 會轉換 "code_verifier"，並將其與 (B) 中的 "t(code_verifier)" 進行比較。  如果兩者不相同，則會拒絕存取。 

#### <a name="faq"></a>常見問題集 
> [!NOTE] 
> 您可能會在 ADFS 系統管理員事件記錄檔中遇到此錯誤：收到不正確 Oauth 要求。 已禁止用戶端 'NAME' 存取範圍為 'ugs' 的資源。 若要補救此錯誤： 
> 1. 啟動 AD FS 管理主控台。 瀏覽至「服務 > 範圍描述」
> 2. 以滑鼠右鍵按一下 [範圍描述]，然後選取 [新增範圍描述]
> 3. 在名稱下輸入 "ugs"，然後按一下 [套用] > [確定]
> 4. 以系統管理員身分執行 Powershell。
> 5. 執行 "Get-AdfsApplicationPermission" 命令。 尋找具有 ClientRoleIdentifier 的 ScopeNames :{openid, aza}。 記下 ObjectIdentifier。
> 6. 執行命令 "Set-AdfsApplicationPermission -TargetIdentifier <步驟 5 的 ObjectIdentifier> -AddScope 'ugs'
> 7. 重新啟動 ADFS 服務。
> 8. 在用戶端上：重新啟動用戶端。 系統應該會提示使用者佈建 WHFB。
> 9. 如果 [佈建] 視窗未出現，則需要收集 NGC 追蹤記錄並執行進一步的疑難排解。

**問：** 我可以將資源值當作範圍值的一部分傳遞，如同對 Azure AD 進行要求一樣？ 
</br>**答：** 使用 Server 2019 上的 AD FS，您現在可以傳遞範圍參數中內嵌的資源值。 範圍參數現在可以組織成以空格分隔的清單，其中的每個項目都會建構為資源/範圍。 例如  
**< 建立有效的範例要求 >**

**問：** AD FS 是否支援 PKCE 擴充功能？
</br>**答：** Server 2019 中的 AD FS 支援 OAuth 授權碼授與流程的 Proof Key for Code Exchange (PKCE) 

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Windows Server 2016 的 Active Directory 同盟服務新功能   
如果您要尋找舊版 AD FS 的相關資訊，請參閱下列文章：  
 [Windows Server 2012 或 2012 R2 中的 ADFS](https://technet.microsoft.com/library/hh831502.aspx) 和 [AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Active Directory 同盟服務在各種不同的應用程式 (包括 Office 365、雲端式 SaaS 應用程式，和公司網路上的應用程式) 上均提供存取控制和單一登入。  
* 對於 IT 組織，此服務可讓您根據同一組認證和原則，對內部部署和雲端中的新式和舊版應用程式提供登入和存取控制。    
* 對於使用者，則會提供採用相同、慣用帳戶憑證的無縫登入。  
* 對於開發人員，此服務提供了簡單的方式可對身分識別位於組織目錄中的使用者進行驗證，讓您得以專注於應用程式，而不是驗證或身分識別。  

本文說明 Windows Server 2016 (AD FS 2016) 中新增的 AD FS 功能。  

## <a name="eliminate-passwords-from-the-extranet"></a>外部網路不使用密碼  
AD FS 2016 啟用了三個新的無需密碼的登入選項，讓組織得以避免因網路釣魚、資料外洩或密碼遭竊而承受網路遭到入侵的風險。 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>使用 Azure Multi-Factor Authentication 登入
AD FS 2016 以 Windows Server 2012 R2 中 AD FS 的多重要素驗證 (MFA) 功能為建置基礎，允許直接使用 Azure MFA 代碼登入，而無須先輸入使用者名稱和密碼。

* 使用 Azure MFA 作為主要驗證方法時，系統會提示使用者輸入其使用者名稱，和 Azure 驗證器應用程式提供的 OTP 代碼。  
* 使用 Azure MFA 作為次要或額外的驗證方法時，使用者必須提供主要的驗證認證 (使用 Windows 整合式驗證、使用者名稱和密碼、智慧卡，或是使用者或裝置憑證)，隨後即會看到文字，語音或 OTP 型 Azure MFA 登入的提示。  
* 使用新的內建 Azure MFA 配接器時，透過 AD FS 進行 Azure MFA 的安裝和設定，將比以往都容易。
* 組織可以利用 Azure MFA，而不需要內部部署 Azure MFA 伺服器。
* Azure MFA 可進行內部網路或外部網路的設定，或作為任何存取控制原則的一部分。

如需 Azure MFA 與 AD FS 搭配使用的詳細資訊
*  [設定 AD FS 2016 和 Azure MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>從相容的裝置進行無密碼存取
AD FS 2016 以先前的裝置註冊功能為建置基礎，啟用了根據裝置合規性狀態的登入和存取控制功能。 使用者可以使用裝置認證登入，而當裝置屬性變更時，即會重新評估合規性，讓您能夠隨時確保原則的強制執行。  這可以啟用如下的原則

* 僅啟用受管理和/或相容的裝置所進行的存取  
* 僅啟用受管理和/或相容的裝置所進行的外部網路存取  
* 未受管理或不符合規範的電腦，必須進行多重要素驗證  

AD FS 在混合式案例中提供條件式存取原則的內部部署元件。 當您使用 Azure AD 註冊裝置以進行雲端資源的條件式存取時，裝置身分識別也可用於 AD FS 原則。

![新功能](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 若要進一步了解如何在雲端中使用以裝置為基礎的條件式存取   
 *  [Azure Active Directory 條件式存取](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

若要進一步了解如何對 AD FS 使用以裝置為基礎的條件式存取
*  [針對 AD FS 規劃以裝置為基礎的條件式存取](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [AD FS 中的存取控制原則](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>使用 Windows Hello 企業版進行登入  

> [!NOTE]
> 目前不支援將 Google Chrome 和[建置於 Chromium 開放原始碼專案瀏覽器的新式 Microsoft Edge](https://www.microsoft.com/edge?form=MB110A&OCID=MB110A) 用於 Microsoft Windows Hello 企業版的瀏覽器單一登入 (SSO)。 請使用 Internet Explorer 或較舊版本的 Microsoft Edge。  

Windows 10 裝置導入了 Windows Hello 和 Windows Hello 企業版，將使用者密碼取代為受到使用者手勢保護的強式裝置繫結使用者認證 (PIN 碼、指紋之類的生物識別手勢或臉部辨識)。 AD FS 2016 支援這些新的 Windows 10 功能，讓使用者無須提供密碼，即可從內部網路或外部網路登入 AD FS 應用程式。

若要進一步了解如何在您的組織中使用 Microsoft Windows Hello 企業版
*  [在組織中啟用 Windows Hello 企業版](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>保護對應用程式的存取

### <a name="modern-authentication"></a>新式驗證
AD FS 2016 支援最新的通訊協定，可針對 Windows 10 以及最新的 iOS 和 Android 裝置與應用程式提供更好的使用者體驗。  

如需詳細資訊，請參閱[適合開發人員使用的 AD FS 案例](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>直接設定存取控制原則而無須了解宣告規則語言  
過去，AD FS 管理員必須使用 AD FS 宣告規則語言來設定原則，因此難以設定和維護原則。 透過存取控制原則，系統管理員可使用內建範本來套用常用的原則，例如
* 僅允許內部網路存取
* 允許所有人，並要求來自外部網路的 MFA
* 允許所有人，並要求特定群組的 MFA

範本可使用精靈驅動的程序輕易自訂，以新增例外狀況或其他原則規則，並且可套用至一或多個應用程式，以強制執行一致的原則。

如需詳細資訊，請參閱 [AD FS 中的存取控制原則](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)。  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>啟用使用非 AD LDAP 目錄的登入  
許多組織都會搭配使用 Active Directory 和第三方目錄。 在新增了對儲存在 LDAP v3 相容目錄中的使用者進行驗證的 AD FS 支援之後，AD FS 現在已可用於：
* 與 LDAP v3 相容的第三方目錄中包含的使用者
* 未設定 Active Directory 雙向信任的 Active Directory 樹系中包含的使用者
* Active Directory 輕量型目錄服務 (AD LDS) 中的使用者

如需詳細資訊，請參閱[設定 AD FS 以驗證 LDAP 目錄中儲存的使用者](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)。  

## <a name="better-sign-in-experience"></a>更好的登入體驗
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>自訂 AD FS 應用程式的登入體驗  
聽您提過，為每個應用程式自訂登入體驗的功能，將是一項絕佳的使用性改進，且若組織提供代表多家不同公司或品牌的應用程式，獲益尤深。  

過去，Windows Server 2012 R2 中的 AD FS 提供了所有信賴憑證者應用程式的一般登入體驗，且能夠自訂個別應用程式的文字內容子集。 透過 Windows Server 2016，您不僅可自訂個別應用程式的訊息，也可自訂其影像、標誌和網頁佈景主題。 此外，您可以建立新的自訂網頁佈景主題，並且就個別的信賴憑證者加以套用。  

如需詳細資訊，請參閱 [AD FS 使用者登入自訂](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)。  



## <a name="manageability-and-operational-enhancements"></a>管理性和操作增強功能  
下一節說明 Windows Server 2016 中的 Active Directory 同盟服務導入的改良操作案例。  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>簡化的審核程序使系統管理更為容易  
在 Windows Server 2012 R2 的 AD FS 中，有許多針對單一要求而產生的稽核事件，但登入或權杖發行活動的相關資訊則付之闕如 (在某些版本的 AD FS 中)，或分散於多個稽核事件間。 AD FS 稽核事件因其資訊龐雜的特性，會預設為關閉。  
隨著 AD FS 2016 的發行，稽核功能變得更精簡且不冗贅。  

如需詳細資訊，請參閱 [Windows Server 2016 中 AD FS 的稽核增強功能](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)。  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>透過 SAML 2.0 改進互通性以參與同盟  
AD FS 2016 包含額外的 SAML 通訊協定支援，包括可根據包含多個實體的中繼資料匯入信任的支援。 這可讓您設定 AD FS 以參與同盟 (例如 InCommon 同盟) 和符合 eGov 2.0 標準的其他實作。  

如需詳細資訊，請參閱[使用 SAML 2.0 改進互通性](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)。  

### <a name="simplified-password-management-for-federated-o365-users"></a>簡化同盟 O365 使用者的密碼管理  
您可以設定 Active Directory 同盟服務 (AD FS)，以將密碼到期宣告傳送至受 AD FS 保護的信賴憑證者信任 (應用程式)。 這些宣告的使用方式取決於應用程式。 例如，若以 Office 365 作為您的信賴憑證者，Exchange 和 Outlook 已實作了相關更新，會在同盟使用者的密碼即將過期時對其發出通知。  

如需詳細資訊，請參閱[設定 AD FS 以傳送密碼到期宣告](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)。  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>從 Windows Server 2012 R2 中的 AD FS 移至 Windows Server 2016 中的 AD FS 比以往更容易  
過去，要移轉至新版本的 AD FS，必須先從舊的伺服器陣列匯出組態，然後再匯入至全新的平行伺服器陣列。  

現在，從 Windows Server 2012 R2 上的 AD FS 移至 Windows Server 2016 上的 AD FS，已比以往容易得多。 只要將新的 Windows Server 2016 伺服器新增至 Windows Server 2012 R2 伺服器陣列，該伺服器陣列就會在 Windows Server 2012 R2 伺服器陣列的行為層級上執行，而使其外觀和行為與 Windows Server 2012 R2 伺服器陣列並無二致。  

然後，將新的 Windows Server 2016 伺服器新增至伺服器陣列，並在驗證功能後，從負載平衡器移除較舊的伺服器。 當所有伺服器陣列節點均執行 Windows Server 2016 之後，您就可以將伺服器陣列行為層級升級至 2016，並開始使用新功能。  

如需詳細資訊，請參閱[升級至 Windows Server 2016 中的 AD FS](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)。  
