---
title: 使用 AD FS 的 OpenID Connect 單一登出
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3af10ec139edbc72e75bf80f544ac5b4f1cf9222
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825769"
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>使用 AD FS 的 OpenID Connect 單一登出

## <a name="overview"></a>總覽
為建置基礎的初始的 Oauth 支援 Windows Server 2012 R2 中的 AD FS 中，AD FS 2016 引進了對 OpenId Connect 登入的支援。 具有[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)，AD FS 2016 現在支援單一登出的 OpenId Connect 的案例。 本文提供 OpenId Connect 案例中的單一記錄檔外的概觀，並提供如何使用它的 OpenId Connect 應用程式在 AD FS 中的指引。


## <a name="discovery-doc"></a>探索文件
OpenID Connect 會使用稱為 「 探索文件 」 的 JSON 文件提供有關組態的詳細資料。  這包括驗證、 權杖、 使用者資訊，以及公用端點的 Uri。  以下是探索文件的範例。

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



表示支援前端通道登出探索文件中將可使用下列的其他值：

- frontchannel_logout_supported： 值將會是 'true'
- frontchannel_logout_session_supported： 值將會是 'true'。
- end_session_endpoint： 這是 OAuth 登出用戶端可以用來起始伺服器上的登出 URI。


## <a name="ad-fs-server-configuration"></a>AD FS 伺服器設定
預設會啟用 AD FS 屬性 EnableOAuthLogout。  此屬性會指示瀏覽的 URL (LogoutURI) 的 AD FS 伺服器，來起始登出用戶端上的 sid。 如果您不需要[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)這是已安裝，您可以使用下列 PowerShell 命令：

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout` 參數會標示為已棄用安裝之後[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)。 `EnableOAUthLogout` 一定會是 true，而且會造成任何影響，登出功能。

>[!NOTE]
>支援 frontchannel_logout**僅**之後安裝的[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>用戶端設定
用戶端必須實作 '登出' 登入使用者的 url。 系統管理員可以使用下列 PowerShell 指令程式的用戶端組態中設定 LogoutUri。 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

`LogoutUri`是 AF FS 用來 「 登出 」 使用者的 url。 實作`LogoutUri`，以確保它必須將用戶端清除使用者在應用程式中的驗證狀態，例如，卸除驗證語彙基元，其。 AD FS 會瀏覽至該 URL，做為查詢參數，發出訊號的信賴憑證者的合作對象的 sid / 應用程式以將使用者登出。 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **與工作階段識別碼的 OAuth 權杖**:AD FS 會包含在 id_token 權杖發行時 OAuth 權杖中的工作階段識別碼。 這將用於稍後由 AD FS 來識別使用者清除相關的 SSO cookie。
2.  **使用者起始登出在 App1 上的**:使用者可以起始從任何已登入的應用程式登出。 在此範例案例中，使用者起始登出，以從 App1。
3.  **應用程式會傳送登出要求給 AD FS**:使用者起始登出之後，應用程式會傳送 GET 要求來 end_session_endpoint 的 AD FS。 應用程式可以選擇性地包含 id_token_hint 做為此要求的參數。 如果 id_token_hint 出現時，AD FS 會使用它搭配工作階段識別碼，找出哪些 URI 用戶端應該重新導向至在登出後 (post_logout_redirect_uri)。  Post_logout_redirect_uri 應該向 AD FS 使用 RedirectUris 參數是有效的 uri。
4.  **AD FS 登入的用戶端傳送登出**:AD FS 使用的工作階段識別碼值來尋找相關的用戶端使用者登入。 識別用戶端會向起始登出用戶端上的 AD FS LogoutUri 上傳送要求。

## <a name="faqs"></a>常見問題集
**問：** 我看不見的 frontchannel_logout_supported 和 frontchannel_logout_session_supported 參數探索文件中。</br>
**答：** 請確定您已[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)所有 AD FS 伺服器上安裝。 單一登出 Server 2016 是指[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)。

**問：** 我已設定單一登出的指示，但使用者保持登入的其他用戶端上。</br>
**答：** 請確認`LogoutUri`設定所有用戶端登入的使用者的所在。 此外，AD FS 會傳送登出要求上的已註冊的最佳嘗試`LogoutUri`。 用戶端必須實作邏輯來處理要求，並採取動作來登出使用者從應用程式。</br>

**問：** 如果在登出後，其中一個用戶端會回到包含有效的重新整理權杖的 AD FS，ADFS 會發出存取權杖？</br>
**答：** 是的。 若要卸除所有經過驗證的成品之後收到的登出要求, 用戶端應用程式負責在已註冊`LogoutUri`。


## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
