---
title: "適用於 OpenID 連接 AD FS 使用的單一登入推出"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3af10ec139edbc72e75bf80f544ac5b4f1cf9222
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2018
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>適用於 OpenID 連接 AD FS 使用的單一登入推出

## <a name="overview"></a>概觀
在的初始 Oauth 支援 AD FS 在 Windows Server 2012 R2 上建置，AD FS 2016 導入了 OpenId 連接登入的支援。 使用[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)，AD FS 2016 現在支援單一登出 OpenId 連接案例。 這篇文章提供 OpenId 連接案例概觀單一登入推出，並提供如何使用 OpenId 連接應用程式中 AD FS 指導方針。


## <a name="discovery-doc"></a>探索文件
連接 OpenID 使用 JSON 文件稱為「探索文件」提供設定的相關詳細資訊。  這包括 Uri 驗證、預付碼、使用者資訊，以及公開-端點。  以下是一個範例，探索文件。

```
{
"issuer":"https://fs.fabidentity.com/adfs",
"authorization_endpoint":"https://fs.fabidentity.com/adfs/oauth2/authorize/",
"token_endpoint":"https://fs.fabidentity.com/adfs/oauth2/token/",
"jwks_uri":"https://fs.fabidentity.com/adfs/discovery/keys",
"token_endpoint_auth_methods_supported":["client_secret_post","client_secret_basic","private_key_jwt","windows_client_authentication"],
"response_types_supported":["code","id_token","code id_token","id_token token","code token","code id_token token"],
"response_modes_supported":["query","fragment","form_post"],
"grant_types_supported":["authorization_code","refresh_token","client_credentials","urn:ietf:params:oauth:grant-type:jwt-bearer","implicit","password","srv_challenge"],
"subject_types_supported":["pairwise"],
"scopes_supported":["allatclaims","email","user_impersonation","logon_cert","aza","profile","vpn_cert","winhello_cert","openid"],
"id_token_signing_alg_values_supported":["RS256"],
"token_endpoint_auth_signing_alg_values_supported":["RS256"],
"access_token_issuer":"http://fs.fabidentity.com/adfs/services/trust",
"claims_supported":["aud","iss","iat","exp","auth_time","nonce","at_hash","c_hash","sub","upn","unique_name","pwd_url","pwd_exp","sid"],
"microsoft_multi_refresh_token":true,
"userinfo_endpoint":"https://fs.fabidentity.com/adfs/userinfo",
"capabilities":[],
"end_session_endpoint":"https://fs.fabidentity.com/adfs/oauth2/logout",
"as_access_token_token_binding_supported":true,
"as_refresh_token_token_binding_supported":true,
"resource_access_token_token_binding_supported":true,
"op_id_token_token_binding_supported":true,
"rp_id_token_token_binding_supported":true,
"frontchannel_logout_supported":true,
"frontchannel_logout_session_supported":true
} 
 
```



將提供探索文件，以指出 Front 通道登出支援下列額外的值：

- frontchannel_logout_supported：值會 true'
- frontchannel_logout_session_supported：值會 true'。
- end_session_endpoint：這是 OAuth 登出 URI client 可供初始化登出伺服器上的。


## <a name="ad-fs-server-configuration"></a>AD FS 伺服器設定
AD FS 屬性 EnableOAuthLogout 將會預設支援。  此屬性的 SID 初始化登出 client 上的指示 AD FS 伺服器瀏覽的 URL (LogoutURI)。 如果您不需要[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)您可以使用下列 PowerShell 命令已安裝：

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout` 參數會標示為過時之後安裝[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)。 `EnableOAUthLogout` 就能為 true，並會在登出功能有任何影響。

>[!NOTE]
>frontchannel_logout 支援**只**之後 installtion 的[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>Client 設定
必須實作的 url '登出' 已登入的使用者，client。 系統管理員可以使用下列 PowerShell cmdlet client 設定中設定 LogoutUri。 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

`LogoutUri`[登出] 使用者 AF FS 使用的 url。 實作的`LogoutUri`、client 需求，確定其清除應用程式中的使用者的授權狀態、卸除驗證，例如權杖它。 AD FS 會瀏覽的 URL，做為查詢參數，成信賴的 sid / 登出使用者的應用程式。 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **工作階段來電顯示與 OAuth 權杖**: AD FS id 工作階段中的 OAuth 權杖包含 id_token 權杖發行的時候。 這將會使用稍後 AD FS 找出使用者清除相關 SSO cookie。
2.  **使用者初始化登出 App1 在**：使用者可以初始化登出從任何已登入應用程式。 在此範例案例中，使用者初始化登出從 App1。
3.  **應用程式將登出要求傳送給 AD FS**︰ 使用者初始化登出之後，應用程式會將的取得要求傳送給 AD fs end_session_endpoint。 應用程式也可以包含 id_token_hint 為的參數，此要求。 如果 id_token_hint，AD FS 將它用於工作階段 ID 搭配圖查看哪些 URI client 應該會重新導向至之後登出 (post_logout_redirect_uri)。  Post_logout_redirect_uri 應該有效的 uri 使用 RedirectUris 參數 AD FS 進行登記完畢。
4.  **AD FS 傳送 sign-out 給戶端登入**: AD FS 使用的工作階段識別碼值，尋找相關戶端使用者登入。 用辨識的戶端初始化登出 client 一邊 AD FS 進行登記 LogoutUri 上傳送要求。

## <a name="faqs"></a>常見問題集
**問：**我看不到 frontchannel_logout_supported 和 frontchannel_logout_session_supported 參數中探索文件。</br>
**A:**確認您[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)安裝的所有 AD FS 伺服器上。 請參考單一登出 Server 2016 以在[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)。

**問：**我設定的單一登出導向，但使用者保持登入其他。</br>
**A:**確認的`LogoutUri`設定為所有戶端所在登入的使用者。 AD FS 而且，會傳送給 sign-out 要求且已的最佳嘗試`LogoutUri`。 Client 必須實作邏輯處理要求和動作來 sign-out 從應用程式的使用者。</br>

**問：**如果之後登出，其中的用回到 AD FS 使用有效的重新整理權杖，將 AD FS 發行存取權杖嗎？</br>
**A:** [是]。 它是 client 應用程式後收到的 sign-out 要求放成品所有已驗證的責任，且已`LogoutUri`。


## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
