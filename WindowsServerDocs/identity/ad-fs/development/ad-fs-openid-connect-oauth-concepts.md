---
title: AD FS OpenID Connect/OAuth 概念
description: 深入瞭解 AD FS 新式驗證概念。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0e680e07ce1ee27a73791e310a71b85ad76d6318
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358756"
---
# <a name="ad-fs-openid-connectoauth-concepts"></a>AD FS OpenID Connect/OAuth 概念
適用于 AD FS 2016 和更新版本
 
## <a name="modern-authentication-actors"></a>新式驗證執行者 

|執行者| 描述|
|-----|-----|
|使用者|這是需要存取資源的安全性主體（使用者、應用程式、服務和群組）。|  
|用戶端|這是您的 web 應用程式，以其用戶端識別碼來識別。 用戶端通常是與使用者互動的合作物件，而且會向授權伺服器要求權杖。
|授權伺服器/識別提供者（IdP）| 這是您的 AD FS 伺服器。 負責驗證存在於組織目錄中的安全性主體的身分識別。 在成功驗證這些安全性主體時，它會發出安全性權杖（持有人存取權杖、識別碼權杖、重新整理權杖）。
|資源伺服器/資源提供者/信賴憑證者| 這是資源或資料所在的位置。 它會信任授權伺服器來安全地驗證和授權用戶端，並使用持有人存取權杖來確保能夠授與資源的存取權。

下圖提供動作專案之間最基本的關聯性：

![新式驗證執行者](media/adfs-modern-auth-concepts/concept1.png)

## <a name="application-types"></a>應用程式類型 
 

|應用程式類型|描述|Role|
|-----|-----|-----|
|原生應用程式|有時也稱為**公用用戶端**，這是要在電腦或裝置上執行的用戶端應用程式，並與使用者互動。|向授權伺服器要求權杖（AD FS），以供使用者存取資源。 使用權杖做為 HTTP 標頭，將 HTTP 要求傳送至受保護的資源。| 
|伺服器應用程式（Web 應用程式）|在伺服器上執行的 web 應用程式，通常可透過瀏覽器存取使用者。 因為它能夠維護自己的用戶端「秘密」或認證，有時也稱為「**機密用戶端**」。 |向授權伺服器要求權杖（AD FS），以供使用者存取資源。 要求權杖之前，用戶端（Web 應用程式）必須使用其密碼進行驗證。 | 
|Web API|使用者所存取的終端資源。 請將這些視為「信賴憑證者」的新標記法。|取用用戶端取得的持有人存取權杖| 

## <a name="application-group"></a>應用程式群組 
 
使用 AD FS 設定的每個 OAuth 用戶端（原生或 web 應用程式）或資源（web api）都必須與應用程式群組相關聯。 應用程式群組中的用戶端可以設定為存取相同群組中的資源。 應用程式群組可以包含多個用戶端和資源。  

## <a name="security-tokens"></a>安全性權杖 
 
新式驗證會使用下列權杖類型： 
- **id_token**：由授權伺服器（AD FS）發出並由用戶端使用的 JWT 權杖。 識別碼權杖中的宣告會包含使用者的相關資訊，讓用戶端可以使用它。  
- **access_token**：由授權伺服器（AD FS）所發行且預定供資源使用的 JWT 權杖。 此 token 的 ' aud ' 或物件宣告必須符合資源或 Web API 的識別碼。  
- **refresh_token**：這是由 AD FS 發出的權杖，供用戶端在需要重新整理 id_token 和 access_token 時使用。 權杖對用戶端而言是不透明的，而且只能由 AD FS 使用。  

## <a name="scopes"></a>任一 
 
在 AD FS 中註冊資源時，可以將範圍設定為允許 AD FS 執行特定的動作。 除了設定範圍之外，也必須在要求中傳送範圍值，以執行動作 AD FS。 例如，在資源註冊期間，系統管理員必須將範圍設定為 openid，而應用程式（用戶端）必須在驗證要求中傳送 scope = openid，以 AD FS 發行識別碼權杖。 以下提供 AD FS 中可用範圍的詳細資料 
 
- aza-如果使用 [適用于訊息代理程式用戶端的 OAuth 2.0 通訊協定延伸](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) 而且範圍參數包含「aza」範圍，則伺服器會發出新的主要重新整理權杖，並將它設定在回應的 [refresh_token] 欄位中，以及將 [refresh_token_expires_in] 欄位設定為新主要重新整理權杖的存留期（如果有強制執行）。 
- openid-允許應用程式要求使用 OpenID Connect 授權通訊協定。 
- logon_cert-logon_cert 範圍可讓應用程式要求登入憑證，以便用來以互動方式登入已驗證的使用者。 AD FS 伺服器會省略回應中的 access_token 參數，並改為提供 base64 編碼的 CMS 憑證鏈或 CMC 完整的 PKI 回應。  [這裡](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e)提供更多詳細資料。
- user_impersonation-必須有 user_impersonation 範圍，才能成功向 AD FS 要求代理者存取權杖。 如需如何使用此範圍的詳細資訊，請參閱[使用 OAuth 搭配 AD FS 2016，使用代理程式（OBO）建立多層式應用程式](ad-fs-on-behalf-of-authentication-in-windows-server.md)。 
- allatclaims – allatclaims 範圍可讓應用程式要求存取權杖中的宣告，以便在識別碼權杖中新增。   
- vpn_cert-vpn_cert 範圍可讓應用程式要求 VPN 憑證，其可用來建立使用 EAP-TLS 驗證的 VPN 連線。 這已不再受到支援。 
- 電子郵件-允許應用程式要求已登入使用者的電子郵件宣告。  
- 設定檔-允許應用程式要求登入使用者的設定檔相關宣告。  

## <a name="claims"></a>宣告 
 
AD FS 發出的安全性權杖（存取和識別碼權杖）包含宣告或已驗證之主體的相關資訊判斷提示。 應用程式可以使用宣告來執行各種工作，包括： 
- 驗證權杖 
- 識別主體的目錄租使用者 
- 顯示使用者資訊 
- 判斷在任何指定的安全性權杖中出現的宣告是否符合主體的授權，取決於權杖類型、用來驗證使用者的認證類型，以及應用程式設定。  
 
## <a name="high-level-ad-fs-authentication-flow"></a>高層級 AD FS 驗證流程 

![AD FS 驗證流程](media/adfs-modern-auth-concepts/adfsauthflow.png)


 1. AD FS 接收來自用戶端的驗證要求。  
 
 2. AD FS 會在驗證要求中驗證用戶端識別碼，並在 AD FS 中的用戶端和資源註冊期間取得用戶端識別碼。 如果使用的是機密用戶端，則 AD FS 也會驗證驗證要求中提供的用戶端密碼。 AD FS 也會驗證用戶端的重新導向 uri。 
 
 3. AD FS 會識別用戶端想要透過在驗證要求中傳遞的資源參數來存取的資源。 如果使用 MSAL 用戶端程式庫，則不會傳送資源參數。 相反地，資源 url 會當做範圍參數的一部分來傳送： *scope = [resource url]//[範圍值，例如 openid]* 。 

    如果資源不是使用資源或範圍參數來傳遞，ADFS 將會使用預設的資源 urn： microsoft：使用者資訊，其原則（例如，MFA、發行或授權原則）無法設定。 
 
 4. 下一個 AD FS 會驗證用戶端是否具有存取資源的許可權。 AD FS 也會驗證傳入驗證要求的範圍是否符合註冊資源時所設定的範圍。 如果用戶端沒有許可權，或未在驗證要求中傳送正確的範圍，則會終止驗證流程。   
 
 5. 一旦驗證許可權和範圍之後，AD FS 使用已設定的[驗證方法](../operations/configure-authentication-policies.md)來驗證使用者。   

 6. 如果根據資源原則或全域驗證原則需要[額外的驗證方法](../operations/configure-additional-authentication-methods-for-ad-fs.md)，AD FS 會觸發額外的驗證。 

 7. AD FS 使用[AZURE mfa](../operations/configure-ad-fs-and-azure-mfa.md)或[協力廠商 mfa](../operations/additional-authentication-methods-ad-fs.md)來執行驗證。   
 
 8. 一旦使用者通過驗證，AD FS 會套用宣告規則（判斷以安全性權杖的一部分傳送至資源的宣告）和[存取控制](../operations/ad-fs-client-access-policies.md)[原則](../deployment/configuring-claim-rules.md)（判斷使用者已符合存取資源所需的條件）。   

 9. 接下來，AD FS 會產生存取和重新整理權杖。 

 10. AD FS 也會產生識別碼權杖。 
 
 11. 如果 scope = allatclaims 包含在驗證要求中，則會根據定義的宣告規則，[自訂識別碼權杖](custom-id-tokens-in-ad-fs.md)以在存取權杖中包含宣告。 
    
 12. 一旦產生並自訂所需的權杖，AD FS 會回應用戶端，包括權杖。 只有在驗證要求包含範圍 = openid 時，識別碼權杖才會包含在回應中。 用戶端一律可以使用權杖端點，取得驗證後的識別碼權杖。 

## <a name="types-of-libraries"></a>程式庫類型 
  
有兩種類型的程式庫會與 AD FS 搭配使用： 
- **用戶端程式庫**：原生用戶端和伺服器應用程式會使用用戶端程式庫來取得存取權杖，以呼叫資源（例如 Web API）。 使用 AD FS 2019 時，Microsoft 驗證程式庫（MSAL）是最新且建議的用戶端程式庫。 Active Directory 驗證程式庫（ADAL）建議用於 AD FS 2016。  

- **伺服器中介軟體程式庫**： Web 應用程式會使用伺服器中介軟體程式庫進行使用者登入。 Web Api 會使用伺服器中介軟體程式庫來驗證原生用戶端或其他伺服器所傳送的權杖。 OWIN （Open Web Interface for .NET）是建議的中介軟體程式庫。 

## <a name="customize-id-token-additional-claims-in-id-token"></a>自訂識別碼權杖（識別碼權杖中的其他宣告）
 
在某些情況下，Web 應用程式（用戶端）可能會需要識別碼權杖中的其他宣告，以協助此功能。 您可以使用下列其中一個選項來達成此目的。 

**選項1：** 當使用公用用戶端，且 web 應用程式沒有嘗試存取的資源時，應該使用。 此選項需要 
1.  response_mode 設定為 form_post 
2.  信賴憑證者識別碼（Web API 識別碼）與用戶端識別碼相同

![AD FS 自訂權杖選項1](media/adfs-modern-auth-concepts/option1.png)

**選項2：** 當 web 應用程式具有其嘗試存取的資源，而且需要透過識別碼權杖傳遞其他宣告時，應該使用。 公用和機密用戶端都可以使用。 此選項需要 
1.  response_mode 設定為 form_post 
2.  KB4019472 安裝在您的 AD FS 伺服器上 
3.  指派給用戶端– RP 配對的範圍 allatclaims。 如下列範例所示，您可以使用 ADFSApplicationPermission （使用已授與一次的 AdfsApplicationPermission） PowerShell Cmdlet 來指派範圍。 

    ``` powershell
    Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
    ```

![AD FS 自訂權杖選項2](media/adfs-modern-auth-concepts/option2.png)

若要進一步瞭解如何在 ADFS 中設定 Web 應用程式以取得自訂的識別碼權杖，請參閱在[使用 OpenID connect 或 OAuth 搭配 AD FS 2016 或更新版本時，自訂要在 id_token 中發出的宣告](Custom-Id-Tokens-in-AD-FS.md)。

## <a name="single-log-out"></a>單一登出

單一登出會導致所有使用會話識別碼的用戶端會話結束。AD FS 2016 和更新版本支援 OpenID Connect/OAuth 的單一登出。 如需詳細資訊，請參閱[使用 AD FS 的 OpenID Connect 單一登出](ad-fs-logout-openid-connect.md)。


## <a name="ad-fs-endpoints"></a>AD FS 端點

|AD FS 端點|描述|
|-----|-----|
|/authorize|AD FS 會傳回可用於取得存取權杖的授權碼|
|/token|AD FS 會傳回存取權杖，可用於存取資源（Web API）|
|/userinfo|AD FS 傳回已驗證使用者的宣告|
|/devicecode|AD FS 會傳回裝置程式碼和使用者程式碼|
|/logout|AD FS 登出使用者|
|/keys|AD FS 用來簽署回應的公開金鑰|
|必須是/.well-known/openid-configuration|AD FS 傳回 OAuth/OpenID Connect 中繼資料|
