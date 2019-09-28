---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: AD FS OpenID Connect/OAuth 流程和應用程式案例
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e1e0235e50945fadd09fe9dd5ffeaf6d7119e482
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385599"
---
# <a name="ad-fs-openid-connectoauth-flows-and-application-scenarios"></a>AD FS OpenID Connect/OAuth 流程和應用程式案例
適用于 AD FS 2016 和更新版本


|狀況|使用範例的案例逐步解說|OAuth 2.0 流量/授與|用戶端類型|
|-----|-----|-----|-----|
|單一頁面應用程式</br> | &bull;[使用 ADAL 的範例](../development/Single-Page-Application-with-AD-FS.md)|[隱](#implicit-grant-flow)|Public| 
|登入使用者的 Web 應用程式</br> | &bull;[使用 OWIN 的範例](../development/enabling-openid-connect-with-ad-fs.md)|[授權碼](#authorization-code-grant-flow)|公用、機密|  
|原生應用程式呼叫 Web API</br>|&bull;[使用 MSAL 的範例](../development/msal/adfs-msal-native-app-web-api.md)</br>&bull;[使用 ADAL 的範例](../development/native-client-with-ad-fs.md)|[授權碼](#authorization-code-grant-flow)|Public|   
|Web 應用程式呼叫 Web API</br>|&bull;[使用 MSAL 的範例](../development/msal/adfs-msal-web-app-web-api.md)</br>&bull;[使用 ADAL 的範例](../development/enabling-oauth-confidential-clients-with-ad-fs.md)|[授權碼](#authorization-code-grant-flow)|機密性| 
|Web API 會代表（OBO）使用者呼叫另一個 Web API</br>|&bull;[使用 MSAL 的範例](../development/msal/adfs-msal-web-api-web-api.md)</br>&bull;[使用 ADAL 的範例](../development/ad-fs-on-behalf-of-authentication-in-windows-server.md)|[代理者](#on-behalf-of-flow)|Web 應用程式的作用為機密| 
|Daemon 應用程式呼叫 Web API||[用戶端認證](#client-credentials-grant-flow)|機密性| 
|Web 應用程式使用使用者認證呼叫 Web API||[資源擁有者密碼認證](#resource-owner-password-credentials-grant-flow-not-recommended)|公用、機密| 
|Browserless 應用程式呼叫 Web API||[裝置程式碼](#device-code-flow)|公用、機密| 

## <a name="implicit-grant-flow"></a>隱含授與流程 
 
若為單一頁面應用程式（AngularJS、Ember.js、回應 .js 等等），AD FS 支援 OAuth 2.0 隱含授與流程。 隱含流程會在 [OAuth 2.0 規格](https://tools.ietf.org/html/rfc6749#section-4.2)中說明。 其主要優點是它可讓應用程式從 AD FS 取得權杖，而不需要執行後端伺服器認證交換。 這可讓應用程式在用戶端 JavaScript 程式碼內，將使用者登入、維護會話，並取得其他 web Api 的權杖。 特別針對 [用戶端](https://tools.ietf.org/html/rfc6749#section-10.3)使用隱含流程時，需要考慮幾個重要的安全性考慮。  
 
如果您想要使用隱含流程和 AD FS 將驗證新增至 JavaScript 應用程式，請遵循下列一般步驟。  
  
### <a name="protocol-diagram"></a>通訊協定圖表

下圖顯示整個隱含登入流程看起來的樣子，接下來的各節會更詳細地說明每個步驟。  

![隱含登入](media/adfs-scenarios-for-developers/implicit.png)

### <a name="request-id-token-and-access-token"></a>要求識別碼權杖和存取權杖 
 
若要一開始將使用者登入您的應用程式，您可以傳送 OpenID Connect 驗證要求，並從 AD FS 端點取得 id_token 和存取權杖。  
 
```
// Line breaks for legibility only 
 
https://adfs.contoso.com/adfs/oauth2/authorize? 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&response_type=id_token+token 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&scope=openid 
&response_mode=fragment 
&state=12345 
```


|參數|必要/選用|描述| 
|-----|-----|-----|
|client_id|必要|AD FS 指派給應用程式的應用程式（用戶端）識別碼。| 
|response_type|必要|必須包含 `id_token`  OpenID connect 登入。 它也可能包含 response_type @ no__t-0。 在這裡使用權杖，可讓您的應用程式立即從授權端點接收存取權杖，而不需要向權杖端點提出第二個要求。| 
|redirect_uri|必要|應用程式的 redirect_uri，其中驗證回應可以由您的應用程式傳送及接收。 它必須完全符合您在 AD FS 中設定的其中一個 redirect_uris。| 
|nonce|必要|包含在要求中的值（由應用程式產生），將會以宣告的形式包含在產生的 id_token 中。 然後, 應用程式可以驗證此值, 以減輕權杖重新執行攻擊。 此值通常是隨機的唯一字串，可以用來識別要求的來源。 只有在要求 id_token 時才需要。|
|scope|選用|以空格分隔的範圍清單。 針對 OpenID Connect，它必須包含範圍 `openid`。|
|resource|選用|Web API 的 url。</br>注意–如果使用 MSAL 用戶端程式庫，則不會傳送資源參數。 相反地，資源 url 會當做範圍參數的一部分來傳送：`scope = [resource url]//[scope values e.g., openid]`</br>如果未在此傳遞資源，或在範圍內，ADFS 會使用預設資源 urn： microsoft：使用者資訊。 您無法自訂 MFA、發行或授權原則等使用者資源原則。| 
|response_mode|選用| 指定應該用來將產生的權杖傳回給應用程式的方法。 預設值為 `fragment`。| 
|state|選用|包含在要求中的值，也會在權杖回應中傳回。 它可以是您想要的任何內容的字串。 隨機產生的唯一值通常用於防止跨網站偽造要求攻擊。 此狀態也可用來在驗證要求發生之前，將使用者狀態的相關資訊編碼，例如他們所在的頁面或觀點。| 
|prompt|選用|表示所需的使用者互動類型。 目前唯一有效的值是 login 和 none。</br>- `prompt=login` 會強制使用者在該要求上輸入其認證，否定單一登入。 </br>- `prompt=none` 相反-它會確保使用者不會看到任何互動式提示。 如果要求無法透過單一登入以無訊息方式完成，AD FS 將會傳回 interaction_required 錯誤。| 
|login_hint|選用|如果您事先知道他們的使用者名稱，可以使用來預先填入使用者登入頁面的使用者名稱/電子郵件地址欄位。 通常應用程式會在重新驗證期間使用此參數，已經從使用 `upn`   `id_token`宣告的先前登入解壓縮使用者名稱。| 
|domain_hint|選用|如果包含，它會略過使用者在登入頁面上經歷的網域型探索程式，進而提高使用者體驗的效率。| 

此時，系統會要求使用者輸入其認證並完成驗證。 一旦使用者通過驗證，AD FS 授權端點就會使用 response_mode 參數中指定的方法，在指定的 redirect_uri 傳回應用程式的回應。  
 
### <a name="successful-response"></a>成功的回應 
 
 `response_mode=fragment and response_type=id_token+token`使用 的成功回應看起來如下所示  
 
```
// Line breaks for legibility only 
 
GET https://localhost/myapp/# 
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZEstZnl0aEV... 
&token_type=Bearer 
&expires_in=3599 
&scope=openid  
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZstZnl0aEV1Q... 
&state=12345 
```


|參數|描述| 
|-----|-----|
|access_token|如果 response_type 包含 @ no__t-0，則包含在內。|
|token_type|如果 response_type 包含 @ no__t-0，則包含在內。 一律會是持有人。| 
|expires_in| 如果 response_type 包含 @ no__t-0，則包含在內。 指出權杖有效的秒數，以供快取之用。| 
|scope| 表示 access_token 將會有效的範圍。|  
|id_token|如果 response_type 包含 @ no__t-0，則包含在內。 帶正負號的 JSON Web 權杖（JWT）。 應用程式可以將此權杖的區段解碼，以要求已登入使用者的相關資訊。 應用程式可以快取並顯示值，但不應依賴它們來取得任何授權或安全性界限。| 
|state|如果要求中包含狀態參數, 則回應中應該會出現相同的值。 應用程式應確認要求和回應中的狀態值完全相同。|

### <a name="refresh-tokens"></a>重新整理權杖 
隱含授與不提供重新整理權杖。  `id_tokens` 和 `access_tokens`會在短時間後過期，因此您的應用程式必須準備好定期重新整理這些權杖。 若要重新整理任一類型的權杖，您可以使用 `prompt=none`  參數來執行相同的隱藏 iframe 要求，以控制身分識別平臺的行為。 如果您想要接收`new id_token`，請務必使用。 `response_type=id_token` 

## <a name="authorization-code-grant-flow"></a>授權碼授與流程 
 
OAuth 2.0 授權碼授與可用於 web 應用程式，以取得受保護資源（例如 web Api）的存取權。 Oauth 2.0 授權碼流程的說明，請參閱 [oauth 2.0 規格的第4.1 節](https://tools.ietf.org/html/rfc6749)。 它是用來在大部分的應用程式類型（包括 web 應用程式和原生安裝的應用程式）中執行驗證和授權。 此流程可讓應用程式安全地取得 access_tokens，以便用來存取 AD FS 信任的資源。  
 
### <a name="protocol-diagram"></a>通訊協定圖表 
 
在高階中，原生應用程式的驗證流程看起來像這樣：

![授權碼授與流程](media/adfs-scenarios-for-developers/authorization.png)

### <a name="request-an-authorization-code"></a>要求授權碼 
 
授權碼流程的開頭是用戶端將使用者導向至/authorize 端點。 在此要求中，用戶端會指出它需要從使用者取得的許可權： 
 
```
// Line breaks for legibility only 
 
https://adfs.contoso.com/adfs/oauth2/authorize? 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&response_type=code 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&response_mode=query 
&resource=https://webapi.com/ 
&scope=openid 
&state=12345 
```

|參數|必要/選用|描述|
|-----|-----|-----| 
|client_id|必要|AD FS 指派給應用程式的應用程式（用戶端）識別碼。|  
|response_type|必要| 必須包含授權碼流程的程式碼。| 
|redirect_uri|必要|應用`redirect_uri`程式的，您的應用程式可以在其中傳送及接收驗證回應。 其必須完全符合您在用戶端的 AD FS 中註冊的其中一個 redirect_uris。|  
|resource|選用|Web API 的 url。</br>注意–如果使用 MSAL 用戶端程式庫，則不會傳送資源參數。 相反地，資源 url 會當做範圍參數的一部分來傳送：`scope = [resource url]//[scope values e.g., openid]`</br>如果未在此傳遞資源，或在範圍內，ADFS 會使用預設資源 urn： microsoft：使用者資訊。 您無法自訂 MFA、發行或授權原則等使用者資源原則。| 
|scope|選用|以空格分隔的範圍清單。|
|response_mode|選用|指定應該用來將產生的權杖傳回給應用程式的方法。 可以是下列其中一項： </br>-query </br>-片段 </br>- form_post</br>`query` 提供程式碼做為重新導向 URI 的查詢字串參數。 如果您要求程式碼，您可以使用 query、片段或 form_post。  `form_post` @ no__t-1executes 一個 POST，其中包含您重新導向 URI 的程式碼。|
|state|選用|包含在要求中的值，也會在權杖回應中傳回。 它可以是您想要的任何內容的字串。 隨機產生的唯一值通常用於防止跨網站偽造要求攻擊。 此值也可以在驗證要求發生之前，在應用程式中將使用者狀態的相關資訊編碼，例如他們所在的頁面或觀點。|
|prompt|選用|表示所需的使用者互動類型。 目前唯一有效的值是 login 和 none。</br>- `prompt=login` 會強制使用者在該要求上輸入其認證，否定單一登入。 </br>- `prompt=none` 相反-它會確保使用者不會看到任何互動式提示。 如果要求無法透過單一登入以無訊息方式完成，AD FS 將會傳回 interaction_required 錯誤。|
|login_hint|選用|如果您事先知道他們的使用者名稱，可以使用來預先填入使用者登入頁面的使用者名稱/電子郵件地址欄位。 通常應用程式會在重新驗證期間使用此參數，已經從使用 `upn` `id_token`宣告的先前登入解壓縮使用者名稱。|
|domain_hint|選用|如果包含，它會略過使用者在登入頁面上經歷的網域型探索程式，進而提高使用者體驗的效率。|
|code_challenge_method|選用|用來將 code_challenge 參數的 code_verifier 編碼的方法。 可為下列其中一個值： </br>-純文字 </br>-S256 </br>如果已排除，則如果包含 @ no__t-0 @ no__t-sp1 是，則會假設 code_challenge 為純文字。 AD FS 同時支援純文字和 S256。 如需詳細資訊，請參閱 [PKCE RFC](https://tools.ietf.org/html/rfc7636)。|
|code_challenge|選用| 用來透過來自 native client 之 Code Exchange （PKCE）的證明金鑰來保護授權碼授與。  `code_challenge_method`如果 包含，則為必要。 如需詳細資訊，請參閱 [PKCE RFC](https://tools.ietf.org/html/rfc7636)|

此時，系統會要求使用者輸入其認證並完成驗證。 一旦使用者通過驗證，AD FS 就會使用 `redirect_uri`  參數 `response_mode`中指定的方法，將回應傳回至指定的應用程式。  
 
### <a name="successful-response"></a>成功的回應 
 
使用 response_mode = query 的成功回應看起來像這樣： 
 
```
GET https://adfs.contoso.com/common/oauth2/nativeclient? 
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq... 
&state=12345  
```


|參數|描述|
|-----|-----|
|code|應用`authorization_code`程式所要求的。 應用程式可以使用授權碼來要求目標資源的存取權杖。 Authorization_codes 的存留期很短，通常會在大約10分鐘後到期。|
|state|如果要求中包含參數,回應中就會出現相同的值。`state` 應用程式應確認要求和回應中的狀態值完全相同。|

### <a name="request-an-access-token"></a>要求存取權杖 
 
既然您已取得`authorization_code`並已授與使用者的許可權，您可以將的程式 `access_token`  代碼兌換成所需的資源。 請將 POST 要求傳送至/token 端點來執行此動作：  
 
```
// Line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com/ 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr... 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&grant_type=authorization_code 
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for confidential clients (web apps)  
```

|參數|必要條件/選擇性|描述|
|-----|-----|-----| 
|client_id|必要|AD FS 指派給應用程式的應用程式（用戶端）識別碼。| 
|grant_type|必要|必須是 `authorization_code`  授權碼流程的。| 
|code|必要|您`authorization_code`在流程的第一個階段中取得的。| 
|redirect_uri|必要|用來`redirect_uri`取得的`authorization_code`相同值。| 
|client_secret|web 應用程式的必要|您在 AD FS 應用程式註冊期間所建立的應用程式密碼。 您不應該在原生應用程式中使用應用程式密碼，因為 client_secrets 無法可靠地儲存在裝置上。 這是 web 應用程式和 web Api 的必要功能，能夠將 client_secret 安全地儲存在伺服器端。 用戶端密碼必須在傳送之前先進行 URL 編碼。 這些應用程式也可以簽署 JWT 並將其新增為 client_assertion 參數，以使用以金鑰為基礎的驗證。| 
|code_verifier|選用|用來取得 authorization_code 的相同 `code_verifier`。 如果在授權碼授與要求中使用 PKCE，則為必要項。 如需詳細資訊，請參閱 [PKCE RFC](https://tools.ietf.org/html/rfc7636)。</br>注意–適用于 AD FS 2019 和更新版本| 

### <a name="successful-response"></a>成功的回應 
 
成功的權杖回應看起來會像這樣: 
 
```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...", 
    "token_type": "Bearer", 
    "expires_in": 3599, 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...", 
} 
```


|參數|描述| 
|-----|-----|
|access_token|要求的存取權杖。 應用程式可以使用此權杖來驗證受保護的資源（Web API）。| 
|token_type|指出 token 類型的值。 唯一 AD FS 支援的類型為持有人。
|expires_in|存取權杖的有效時間長度 (以秒為單位)。
|refresh_token|OAuth 2.0 重新整理權杖。 應用程式可以使用此權杖，在目前的存取權杖到期之後取得其他存取權杖。 Refresh_tokens 是長時間存留的，可用來長期保留資源的存取權。| 
|refresh_token_expires_in|重新整理權杖的有效時間長度（以秒為單位）。| 
|id_token|JSON Web 權杖（JWT）。 應用程式可以將此權杖的區段解碼，以要求已登入使用者的相關資訊。 應用程式可以快取並顯示值，但不應依賴它們來取得任何授權或安全性界限。|

### <a name="use-the-access-token"></a>使用存取權杖 
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q... 
 ```

### <a name="refresh-the-access-token"></a>重新整理存取權杖 
 
Access_tokens 的存留期很短，而且您必須在過期後重新整理，才能繼續存取資源。 若要這麼做，您可以向 @ no__t-0 @ no__t-1endpoint 提交另一個 POST 要求，這次提供 refresh_token，而不是程式碼。 重新整理權杖對您的用戶端已收到存取權杖的擁有權限都是有效的。 
 
重新整理權杖沒有指定的存留期。 一般而言，重新整理權杖的存留期相對較長。 不過，在某些情況下，重新整理權杖會過期、遭到撤銷，或缺少所需動作的足夠許可權。 您的應用程式必須預期並正確處理權杖發行端點所傳回的錯誤。  
 
雖然重新整理權杖不會在用來取得新的存取權杖時撤銷，但您仍應捨棄舊的重新整理權杖。 OAuth 2.0 規格指出：「授權伺服器可能會發出新的重新整理權杖，在此情況下，用戶端必須捨棄舊的重新整理權杖，並以新的重新整理權杖取代。 授權伺服器可能會在發出新的重新整理權杖給用戶端之後，撤銷舊的重新整理權杖。」 
 
```
// Line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq... 
&grant_type=refresh_token 
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for confidential clients (web apps)  
```


|參數|必要/選用|描述| 
|-----|-----|-----|
|client_id|必要|AD FS 指派給應用程式的應用程式（用戶端）識別碼。| 
|grant_type|必要|必須是 `refresh_token`  授權碼流程的這個階段。| 
|resource|選用|Web API 的 url。</br>注意–如果使用 MSAL 用戶端程式庫，則不會傳送資源參數。 相反地，資源 url 會當做範圍參數的一部分來傳送：`scope = [resource url]//[scope values e.g., openid]`</br>如果未在此傳遞資源，或在範圍內，ADFS 會使用預設資源 urn： microsoft：使用者資訊。 您無法自訂 MFA、發行或授權原則等使用者資源原則。|
|scope|選用|以空格分隔的範圍清單。| 
|refresh_token|必要|您在流程的第二個階段中取得的 refresh_token。| 
|client_secret|web 應用程式的必要| 您在應用程式註冊入口網站中為應用程式建立的應用程式密碼。 不應在原生應用程式中使用，因為 client_secrets 無法可靠地儲存在裝置上。 這是 web 應用程式和 web Api 的必要功能，能夠將 client_secret 安全地儲存在伺服器端。 這些應用程式也可以簽署 JWT 並將其新增為 client_assertion 參數，以使用以金鑰為基礎的驗證。|

### <a name="successful-response"></a>成功的回應 
成功的權杖回應看起來會像這樣: 
 
```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...", 
    "token_type": "Bearer", 
    "expires_in": 3599, 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...", 
}  
```
|參數|描述| 
|-----|-----|
|access_token|要求的存取權杖。 應用程式可以使用此權杖來驗證受保護的資源，例如 Web API。| 
|token_type|指出 token 類型的值。 唯一 AD FS 支援的類型為持有人|
|expires_in|存取權杖的有效時間長度 (以秒為單位)。|
|scope|Access_token 有效的範圍。| 
|refresh_token|OAuth 2.0 重新整理權杖。 應用程式可以使用此權杖，在目前的存取權杖到期之後取得其他存取權杖。 Refresh_tokens 是長時間存留的，可用來長期保留資源的存取權。| 
|refresh_token_expires_in|重新整理權杖的有效時間長度（以秒為單位）。| 
|id_token|JSON Web 權杖（JWT）。 應用程式可以將此權杖的區段解碼，以要求已登入使用者的相關資訊。 應用程式可以快取並顯示值，但不應依賴它們來取得任何授權或安全性界限。|

## <a name="on-behalf-of-flow"></a>代理者流程 
 
OAuth 2.0 代理者流程（OBO）提供應用程式叫用服務/Web API 的使用案例，而這又需要呼叫另一個服務/Web API。 其概念是透過要求鏈傳播委派的使用者身分識別和許可權。 若要讓仲介層服務向下游服務提出已驗證的要求，它需要代表使用者保護來自 AD FS 的存取權杖。  
 
### <a name="protocol-diagram"></a>通訊協定圖表 
假設使用者已使用上述的 OAuth 2.0 授權碼授與流程在應用程式上進行驗證。 此時，應用程式有一個適用于 API A 的存取權杖（權杖 A），其中包含使用者的宣告，並同意存取仲介層 Web API （API A）。 請確定用戶端要求權杖中的 user_impersonation 範圍。 現在，API A 必須向下游 Web API （API B）提出已驗證的要求。 

接下來的步驟構成 OBO 流程，並透過下圖的協助加以說明。 

![代理者流程](media/adfs-scenarios-for-developers/obo.png)

  1. 用戶端應用程式使用權杖 A 向 API A 提出要求。  
  注意：在 AD FS 中設定 OBO 流程時，請`user_impersonation`確定已選取範圍，且`user_impersonation`要求中的用戶端要求範圍。 
  2. API A 會向 AD FS 權杖發行端點進行驗證，並要求權杖以存取 API B。注意：在 AD FS 中設定此流程時，請確定 API A 也已註冊為伺服器應用程式，而 clientID 的值與 API A 中的資源識別碼相同。如需詳細資訊，請參閱這裡的範例新增連結。  
  3. AD FS 權杖發行端點會使用權杖 A 來驗證 API A 的認證，併發出 API B （token B）的存取權杖。 
  4. 權杖 B 在對 API B 的要求的授權標頭中設定。 
  5. API B 傳回來自受保護資源的資料。 

### <a name="service-to-service-access-token-request"></a>服務對服務存取權杖要求 
 
若要要求存取權杖，請使用下列參數對 AD FS token 端點提出 HTTP POST。  


### <a name="first-case-access-token-request-with-a-shared-secret"></a>第一種情況:具有共用密碼的存取權杖要求 
 
使用共用密碼時, 服務對服務存取權杖要求包含下列參數: 


|參數|必要/選用|描述|
|-----|-----|-----| 
|grant_type|必要|權杖要求的類型。 針對使用 JWT 的要求，此值必須為 urn： ietf： params： oauth： grant-type： JWT-持有人。|  
|client_id|必要|將您的第一個 Web API 註冊為伺服器應用程式時所設定的用戶端識別碼（仲介層應用程式）。 這應該與第一個階段中使用的資源識別碼相同，亦即第一個 Web API 的 url。| 
|client_secret|必要|您在 AD FS 中的伺服器應用程式註冊期間所建立的應用程式密碼。| 
|確定性|必要|在要求中使用的 token 值。|  
|requested_token_use|必要|指定應該如何處理要求。 在 OBO 流程中，此值必須設定為 on_behalf_of| 
|resource|必要|將第一個 Web API 註冊為伺服器應用程式（仲介層應用程式）時所提供的資源識別碼。 資源識別碼應為第二個 Web API 中介層應用程式的 url，以代表用戶端呼叫。|
|scope|選用|權杖要求範圍的空格分隔清單。| 

#### <a name="example"></a>範例 
 
下列`HTTP POST`要求存取權杖和重新整理權杖 
 
```
//line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: adfs.contoso.com  
Content-Type: application/x-www-form-urlencoded 
 
grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer 
&client_id=https://webapi.com/ 
&client_secret=BYyVnAt56JpLwUcyo47XODd 
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIm… 
&resource=https://secondwebapi.com/
&requested_token_use=on_behalf_of
&scope=openid    
```

### <a name="second-case-access-token-request-with-a-certificate"></a>第二個案例:具有憑證的存取權杖要求 
 
具有憑證的服務對服務存取權杖要求包含下列參數: 


|參數|必要/選用|描述|
|-----|-----|-----| 
|grant_type|必要|權杖要求的類型。 針對使用 JWT 的要求，此值必須為 urn： ietf： params： oauth： grant-type： JWT-持有人。 |
|client_id|必要|將您的第一個 Web API 註冊為伺服器應用程式時所設定的用戶端識別碼（仲介層應用程式）。 這應該與第一個階段中使用的資源識別碼相同，亦即第一個 Web API 的 url。|  
|client_assertion_type|必要|此值必須為 urn： ietf： params： oauth：用戶端-判斷提示類型： jwt-持有人。| 
|client_assertion|必要|您需要建立的判斷提示（JSON web 權杖），並使用您註冊為應用程式認證的憑證來簽署。|  
|確定性|必要|在要求中使用的 token 值。| 
|requested_token_use|必要|指定應該如何處理要求。 在 OBO 流程中，此值必須設定為 on_behalf_of| 
|resource|必要|將第一個 Web API 註冊為伺服器應用程式（仲介層應用程式）時所提供的資源識別碼。 資源識別碼應為第二個 Web API 中介層應用程式的 url，以代表用戶端呼叫。|
|scope|選用|權杖要求範圍的空格分隔清單。|


請注意，參數與共享密碼要求的情況幾乎相同，不同之處在于 client_secret 參數是由兩個參數所取代： client_assertion_type 和 client_assertion。 

#### <a name="example"></a>範例 
下列 HTTP POST 會使用憑證要求 Web API 的存取權杖。

``` 
// line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer 
&client_id= https://webapi.com/ 
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer 
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNS… 
&resource=https://secondwebapi.com/
&requested_token_use=on_behalf_of
&scope= openid 
```    

### <a name="service-to-service-access-token-response"></a>服務對服務存取權杖回應 
 
成功回應是 JSON OAuth 2.0 回應，具有下列參數。 


|參數|描述|
|-----|-----| 
|token_type|指出 token 類型的值。 唯一 AD FS 支援的類型為持有人。 | 
|scope|在權杖中授與的存取範圍。| 
|expires_in|存取權杖有效的時間長度（以秒為單位）。| 
|access_token|要求的存取權杖。 呼叫端服務可以使用此權杖來驗證接收服務。| 
|id_token|JSON Web 權杖（JWT）。 應用程式可以將此權杖的區段解碼，以要求已登入使用者的相關資訊。 應用程式可以快取並顯示值，但不應依賴它們來取得任何授權或安全性界限。| 
|refresh_token|所要求之存取權杖的重新整理權杖。 呼叫的服務可以使用此權杖, 在目前的存取權杖到期後要求另一個存取權杖。|
|Refresh_token_expires_in|重新整理權杖有效的時間長度（以秒為單位）。 

### <a name="success-response-example"></a>成功回應範例 
 
下列範例顯示 Web API 的存取權杖要求的成功回應。 

``` 
{ 
  "token_type": "Bearer", 
  "scope": openid, 
  "expires_in": 3269, 
  "access_token": "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1t" 
  "id_token": "aWRfdG9rZW49ZXlKMGVYQWlPaUpLVjFRaUxDSmhiR2NpT2lKU1V6STFOa" 
  "refresh_token": "OAQABAAAAAABnfiG…" 
  "refresh_token_expires_in": 28800, 
} 
```  
 
 
使用存取權杖來存取受保護的資源現在仲介層服務可以使用上面取得的權杖，藉由在 Authorization 標頭中設定權杖，向下游 Web API 提出已驗證的要求。  

#### <a name="example"></a>範例 
``` 
GET /v1.0/me HTTP/1.1 
Host: https://secondwebapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQ… 
``` 

## <a name="client-credentials-grant-flow"></a>用戶端認證授與流程 
 
您可以使用[RFC 6749](https://tools.ietf.org/html/rfc6749#section-4.4)中指定的 OAuth 2.0 用戶端認證授與，利用應用程式的身分識別來存取 web 裝載的資源。 這種類型的授與通常用於必須在背景中執行的伺服器對伺服器互動，而不必立即與使用者互動。 這些類型的應用程式通常稱為「守護程式」或「服務帳戶」。 

OAuth 2.0 用戶端認證授與流程可允許 web 服務（機密用戶端）在呼叫另一個 web 服務時，使用自己的認證來進行驗證，而不是模擬使用者。 在此案例中，用戶端通常是仲介層 web 服務、daemon 服務或網站。 為了獲得更高的保證，AD FS 也允許呼叫服務使用憑證（而非共用密碼）做為認證。 

### <a name="protocol-diagram"></a>通訊協定圖表 

下圖顯示用戶端認證授與流程。 

![用戶端認證](media/adfs-scenarios-for-developers/credentials.png)

### <a name="request-a-token"></a>要求權杖 
 
若要使用用戶端認證授與來取得權杖，請`POST`將要求傳送至/token AD FS 端點：  
 
### <a name="first-case-access-token-request-with-a-shared-secret"></a>第一種情況:具有共用密碼的存取權杖要求 
 
```
POST /adfs/oauth2/token HTTP/1.1            
//Line breaks for clarity 
 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
client_id=535fb089-9ff3-47b6-9bfb-4f1264799865 
&client_secret=qWgdYAmab0YSkuL1qKv5bPX 
&grant_type=client_credentials 
```

|參數|必要/選用|描述|
|-----|-----|-----| 
|client_id|必要|AD FS 指派給應用程式的應用程式（用戶端）識別碼。| 
|scope|選用|您想要使用者同意的範圍清單（以空格分隔）。| 
|client_secret|必要|您在應用程式註冊入口網站中為應用程式產生的用戶端密碼。 用戶端密碼必須在傳送之前先進行 URL 編碼。| 
|grant_type|必要|必須設定為 `client_credentials`。|

### <a name="second-case-access-token-request-with-a-certificate"></a>第二個案例:具有憑證的存取權杖要求 

``` 
POST /adfs/oauth2/token HTTP/1.1                
 
// Line breaks for clarity 
 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05 
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer 
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg 
&grant_type=client_credentials  
```

|參數|必要/選用|描述| 
|-----|-----|-----|
|client_assertion_type|必要|此值必須設定為 urn： ietf： params： oauth：用戶端-判斷提示類型： jwt-持有人。| 
|client_assertion|必要|您需要建立的判斷提示（JSON web 權杖），並使用您註冊為應用程式認證的憑證來簽署。|  
|grant_type|必要|必須設定為 `client_credentials`。|
|client_id|選用|AD FS 指派給應用程式的應用程式（用戶端）識別碼。 這是 client_assertion 的一部分，因此不需要在此傳入。| 
|scope|選用|您想要使用者同意的範圍清單（以空格分隔）。| 

### <a name="use-a-token"></a>使用權杖 
 
既然您已取得權杖，請使用權杖來對資源提出要求。 當令牌過期時，請對/token 端點重複要求，以取得全新的存取權杖。  
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...  
```

## <a name="resource-owner-password-credentials-grant-flow-not-recommended"></a>資源擁有者密碼認證授與流程（不建議） 
 
資源擁有者密碼認證（ROPC）授與可讓應用程式直接處理其密碼來登入使用者。 ROPC 流程需要高程度的信任和使用者暴露，而且您應該只在有其他、更安全的流程無法使用時，才使用此流程。  
 
### <a name="protocol-diagram"></a>通訊協定圖表 
 
下圖顯示 ROPC 流程。

![ROPC 流程](media/adfs-scenarios-for-developers/resource.png)

### <a name="authorization-request"></a>授權要求 
ROPC 流程是單一要求，它會將用戶端識別和使用者的認證傳送至 IDP，然後接收傳回的權杖。 在這麼做之前，用戶端必須要求使用者的電子郵件地址（UPN）和密碼。 成功要求之後，用戶端應該會在記憶體中安全地釋放使用者的認證。 它絕不能儲存它們。  

```
// Line breaks and spaces are for legibility only. 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com  
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&scope= openid  
&username=myusername@contoso.com 
&password=SuperS3cret 
&grant_type=password 
```


|參數|必要/選用|描述| 
|-----|-----|-----|
|client_id|必要|用戶端識別碼| 
|grant_type|必要|必須設定為 [密碼]。| 
|username|必要|使用者的電子郵件地址。| 
|password|必要|使用者的密碼。| 
|scope|選用|以空格分隔的範圍清單。|

### <a name="successful-authentication-response"></a>成功的驗證回應 
下列範例會顯示成功的權杖回應： 

```
{ 
    "token_type": "Bearer", 
    "scope": "openid", 
    "expires_in": 3599, 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIn...", 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDR..." 
}  
```


|參數|描述| 
|-----|-----|
|token_type|一律設定為 [持有人]。| 
|scope|如果傳回存取權杖，此參數會列出存取權杖有效的範圍。| 
|expires_in|包含的存取權杖有效的秒數。| 
|access_token|已針對要求的範圍發出。| 
|id_token|JSON Web 權杖（JWT）。 應用程式可以將此權杖的區段解碼，以要求已登入使用者的相關資訊。 應用程式可以快取並顯示值，但不應依賴它們來取得任何授權或安全性界限。| 
|refresh_token_expires_in|內含的重新整理權杖有效的秒數。| 
|refresh_token|如果原始範圍參數包含 offline_access，則發出。|

您可以使用重新整理權杖來取得新的存取權杖，並使用上述驗證碼授與流程一節中所述的相同流程來重新整理權杖。   

## <a name="device-code-flow"></a>裝置程式碼流程 
 
裝置程式碼授與可讓使用者登入受輸入限制的裝置，例如智慧型電視、IoT 裝置或印表機。 若要啟用此流程, 裝置會讓使用者在其他裝置的瀏覽器中造訪網頁以進行登入。 使用者登入後, 裝置就能夠視需要取得存取權杖和重新整理權杖。 
 
### <a name="protocol-diagram"></a>通訊協定圖表 
 
整個裝置程式碼流程看起來類似下圖。 我們會在本文稍後說明每個步驟。 
 
![裝置程式碼流程](media/adfs-scenarios-for-developers/device.png)

### <a name="device-authorization-request"></a>裝置授權要求 
用戶端必須先向驗證服務器檢查用來起始驗證的裝置和使用者程式碼。 用戶端會從/devicecode 端點收集此要求。 在此要求中, 用戶端也應該包含從使用者取得所需的許可權。 從傳送此要求開始，使用者只有15分鐘的時間來登入（expires_in 的一般值），因此只會在使用者已指出他們已準備好登入時，才提出此要求。 

```
// Line breaks are for legibility only. 
 
POST https://adfs.contoso.com/adfs/oauth2/devicecode 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
scope=openid 
```


|參數|條件|描述|
|-----|-----|-----| 
|client_id|必要|AD FS 指派給應用程式的應用程式（用戶端）識別碼。| 
|scope|選用|以空格分隔的範圍清單。|

### <a name="device-authorization-response"></a>裝置授權回應 
成功的回應會是 JSON 物件, 其中包含允許使用者登入的必要資訊。 


|參數|描述|
|-----|-----| 
|device_code|長字串, 用來驗證用戶端與授權伺服器之間的會話。 用戶端會使用此參數來向授權伺服器要求存取權杖。| 
|user_code|向使用者顯示的簡短字串, 用來識別次要裝置上的會話。| 
|verification_uri|使用者應該移至 user_code 以便登入的 URI。| 
|verification_uri_complete|使用者應該移至 user_code 以便登入的 URI。 這會預先填入 user_code，讓使用者不需要輸入 user_code| 
|expires_in|Device_code 和 user_code 到期之前的秒數。| 
|期間|用戶端應在輪詢要求之間等待的秒數。| 
|message|人類看得懂的字串, 其中包含使用者的指示。 這可以藉由在要求中包含查詢參數來當地語系化，其格式為？ mkt = xx-XX，填入適當的語言文化特性代碼。  

### <a name="authenticating-the-user"></a>驗證使用者 
收到 user_code 和 verification_uri 之後，用戶端會向使用者顯示這些專案，指示他們使用其行動電話或電腦瀏覽器登入。 此外，用戶端可以使用 QR 代碼或類似的機制來顯示 verfication_uri_complete，這會採取步驟來進入使用者的 user_code。 當使用者在 verification_uri 進行驗證時，用戶端應該使用 device_code 輪詢所要求權杖的/token 端點。 

```
POST https://adfs.contoso.com /adfs/oauth2/token 
Content-Type: application/x-www-form-urlencoded 
 
grant_type: urn:ietf:params:oauth:grant-type:device_code 
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e 
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8 
```


|參數|必要|描述|
|-----|-----|-----| 
|grant_type|必要|必須是 urn： ietf： params： oauth： grant-type： device_code| 
|client_id|必要|必須符合初始要求中使用的 client_id。| 
|code|必要|裝置授權要求中傳回的 device_code。|

### <a name="successful-authentication-response"></a>成功的驗證回應 
成功的權杖回應看起來會像這樣:  


|參數|描述|
|-----|-----| 
|token_type|一律為「持有人」。| 
|scope|如果傳回存取權杖, 這會列出存取權杖有效的範圍。| 
|expires_in|所包含存取權杖有效的秒數。| 
|access_token|已針對要求的範圍發出。| 
|id_token|如果原始範圍參數包含 openid 範圍，則發出。| 
|refresh_token|如果原始範圍參數包含 offline_access，則發出。| 
|refresh_token_expires_in|內含的重新整理權杖有效前的秒數。| 


## <a name="related-content"></a>相關內容  
如需逐步解說文章的完整清單，其中提供使用相關流程的逐步指示，請參閱[AD FS 開發](../AD-FS-Development.md)。 
