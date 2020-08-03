---
title: 使用 AD FS 的 OpenID Connect 單一登出
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7821910caa3c0cfa5c5402df57bd758ce8d0c245
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519867"
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>使用 AD FS 的 OpenID Connect 單一登出

## <a name="overview"></a>概觀
AD FS 2016 以 Windows Server 2012 R2 AD FS 中的初始 Oauth 支援為基礎，引進了 OpenId Connect 登入的支援。 透過[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)，AD FS 2016 現在支援 OpenId connect 案例的單一登出。 本文概述 OpenId Connect 的單一登出案例，並提供如何在 AD FS 中將它用於 OpenId Connect 應用程式的指引。


## <a name="discovery-doc"></a>探索檔
OpenID Connect 會使用稱為「探索檔」的 JSON 檔來提供設定的詳細資料。  這包括驗證、權杖、使用者資訊和公用端點的 Uri。  以下是探索檔的範例。

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



探索檔中會提供下列額外的值，表示支援前端通道登出：

- frontchannel_logout_supported：值會是 ' true '
- frontchannel_logout_session_supported：值會是 ' true '。
- end_session_endpoint：這是用戶端可以用來在伺服器上起始登出的 OAuth 登出 URI。


## <a name="ad-fs-server-configuration"></a>AD FS 伺服器設定
預設會啟用 AD FS 屬性 EnableOAuthLogout。  此屬性會指示 AD FS 伺服器流覽具有 SID 的 URL （LogoutURI），以起始用戶端上的登出。
如果您尚未安裝[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) ，您可以使用下列 PowerShell 命令：

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout`安裝[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)之後，參數會標示為過時。 `EnableOAUthLogout`一定會是 true，而且不會影響登出功能。

>[!NOTE]
>只有在安裝[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)之後**才**支援 frontchannel_logout

## <a name="client-configuration"></a>用戶端組態
用戶端必須執行「登出」已登入使用者的 url。 系統管理員可以使用下列 PowerShell Cmdlet，在用戶端設定中設定 LogoutUri。


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

`LogoutUri`是 AF FS 用來「登出」使用者的 url。 `LogoutUri`若要執行，用戶端必須確保它會清除應用程式中使用者的驗證狀態，例如，卸載它所擁有的驗證權杖。 AD FS 將流覽至該 URL，並以 SID 作為查詢參數，以通知信賴憑證者/應用程式登出使用者。

![ADFS 登出使用者圖表](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)

1.  **具有會話識別碼**： AD FS 的 oauth 權杖會在 id_token 權杖發行時，于 OAuth 權杖中包含會話識別碼。 稍後會使用此 AD FS 來識別要為使用者清除的相關 SSO cookie。
2.  **使用者會在 App1 上起始登出**：使用者可以從任何已登入的應用程式起始登出。 在此範例案例中，使用者會從 App1 起始登出。
3.  **應用程式將登出要求傳送至 AD FS**：在使用者起始登出之後，應用程式會將 GET 要求傳送至 AD FS 的 end_session_endpoint。 應用程式可以選擇性地包含 id_token_hint 做為此要求的參數。 如果 id_token_hint 存在，AD FS 會將其與會話識別碼搭配使用，以找出用戶端在登出後應重新導向至哪個 URI （post_logout_redirect_uri）。  Post_logout_redirect_uri 應該是使用 RedirectUris 參數向 AD FS 註冊的有效 uri。
4.  **AD FS 會將登出傳送至登入的用戶端**： AD FS 會使用會話識別碼值來尋找使用者所登入的相關用戶端。 已識別的用戶端會在向 AD FS 註冊的 LogoutUri 上傳送要求，以起始用戶端上的登出。

## <a name="faqs"></a>常見問題集
**問：** 我在探索檔中看不到 frontchannel_logout_supported 和 frontchannel_logout_session_supported 參數。</br>
**答：** 請確定您已在所有 AD FS 伺服器上安裝[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) 。 請參閱使用[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)在伺服器2016中的單一登出。

**問：** 我已依照指示設定單一登出，但使用者在其他用戶端上保持登入狀態。</br>
**答：** 請確定 `LogoutUri` 已針對使用者登入的所有用戶端設定。 此外，AD FS 會在已註冊的上嘗試傳送登出要求的最佳案例 `LogoutUri` 。 用戶端必須執行邏輯來處理要求，並採取動作將使用者從應用程式登出。</br>

**問：** 如果登出之後，其中一個用戶端回到 AD FS 具有有效的重新整理權杖，就會 AD FS 發行存取權杖嗎？</br>
**答：** 是。 用戶端應用程式必須負責在註冊後收到登出要求之後，捨棄所有已驗證 `LogoutUri` 的成品。


## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)
