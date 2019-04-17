---
title: "自訂來使用 OpenID 連接或 OAuth AD FS 2016 id_token 發出宣告"
description: "AD FS 2016 中自訂 id 權杖 conecpts 技術概觀"
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: c17d50bb6d90726eafc3e585f16a7a8189a68392
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2018
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016"></a>自訂來使用 OpenID 連接或 OAuth AD FS 2016 id_token 發出宣告

## <a name="overview"></a>概觀
文章[在此](enabling-openId-connect-with-ad-fs.md)顯示如何建置的應用程式的 OpenID 連接登入，請使用 AD FS。 不過，預設不會修正的設定 id_token 提供索賠項目。 AD FS 2016 有自訂 id_token OpenID 連接案例中的功能。

## <a name="when-are-custom-id-token-used"></a>何時自訂 ID 權杖使用嗎？
在某些案例很可能 client 應用程式不會有嘗試存取資源。 因此，就不需要存取預付碼。 此時，client 應用程式基本上需要只 ID 權杖，但有一些其他宣告協助功能。

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>取得 ID 的自訂宣告權杖限制為何？

### <a name="scenario-1"></a>案例 1

![限制](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode 設定為 form_post
2.  只公開戶端可以取得自訂宣告 ID 權杖
3.  信賴的派對識別字應相同 client id

### <a name="scenario-2"></a>案例 2

![限制](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

使用[KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472)您 AD FS 伺服器上安裝
1.  response_mode 設定為 form_post
2.  將範圍 allatclaims 指派給 client – 資源點數配對。
您可以指定範圍使用 Grant-ADFSApplicationPermission cmdlet 以下範例所示：

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-an-oauth-application-to-handle-custom-claims-in-id-token"></a>建立處理自訂宣告 ID 權杖中的 OAuth 應用程式
使用文章[在此](Enabling-OpenId-Connect-with-AD-FS-2016.md)來建立所使用的應用程式應用程式的 OpenID 連接 AD FS 登入。 然後，依照下列步驟來接收 ID 權杖自訂宣告應用程式設定中 AD FS。

### <a name="create-the-application-group-in-ad-fs-2016"></a>AD FS 2016 中建立應用程式群組

1.  建立稱為 CustomTokenClient 新範本，如下所示，為基礎的應用程式群組。

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

2. 此範本建立機密 client。 請注意識別碼，並傳回 URI 指定與專案 SSL url。

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

3.  在下一個步驟中，選取 [**產生共用的密碼**來建立 client 認證及複製產生 client 認證。

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

4. 按一下**下一步**，然後繼續進行完成精靈。

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

### <a name="create-the-relying-party"></a>建立信賴
為了將自訂宣告新增 ID 權杖，您需要建立 ID 權杖中會新增其宣告資源點數。 使用 [新增可以廠商信任精靈建立新的信賴，如下所示：
 
![信賴](media/Custom-Id-Tokens-in-AD-FS/rpsnap1.png)

建立信賴之後，信賴廠商項目上按一下滑鼠右鍵，然後選取**編輯宣告 \ [發行原則**來新增宣告發行規則。 加入權杖 ID 需要自訂宣告如下：

![信賴](media/Custom-Id-Tokens-in-AD-FS/rpsnap2.png)

### <a name="assign-allatclaims-scope-to-the-pair-of-client-and-relying-party"></a>將「allatclaims「範圍指派給 client 與信賴的配對
AD FS 在伺服器上，使用 PowerShell 指派為以下範例中指定新 allatclaims 領域 (變更 clientID 和 server:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "5db77ce4-cedf-4319-85f7-cc230b7022e0" -ServerRoleIdentifier "https://customidrp1/" -ScopeNames "allatclaims","openid"
```

>[!NOTE]
>根據您的應用程式設定變更 ClientRoleIdentifier 和 ServerRoleIdentifier

## <a name="test-the-custom-claims-in-id-token"></a>測試 ID 權杖自訂宣告

然後，使用的程式碼，您隨時存取主張使用相同的位元，您就可以看到將會變成 id_token 部分的額外宣告。
例如，.NET MVC 範例應用程式中打開其中一個控制器檔案，並輸入驗證碼，例如如下：


``` code
    [Authorize]
    public ActionResult About()
    {

        ClaimsPrincipal cp = ClaimsPrincipal.Current;

        string userName = cp.FindFirst(ClaimTypes.GivenName).Value;
        ViewBag.Message = String.Format("Hello {0}!", userName);
        return View();

    }

```

>[!NOTE]
>請留意資源參數，則必須為 Oauth2 要求中。
>
>錯誤範例：
>
>**Https 和 #58; 日日 sts.contoso.com/adfs/oauth2/authorize?response_type=id_token&scope=openid&redirect_uri=https&#58;//myportal/auth&nonce=b3ceb943fc756d927777&client_id=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7&state=42c2c156aef47e8d0870&resource=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7**
>
>告訴您一個好範例：
>
>**Https 和 #58; 日日 sts.contoso.com/adfs/oauth2/authorize?response_type=id_token&scope=openid&redirect_uri=https&#58;//myportal/auth&nonce=b3ceb943fc756d927777&client_id=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7&state=42c2c156aef47e8d0870&resource=https&#58;//customidrp1/**
>
>如果資源參數不在要求您可能會收到錯誤或預付碼，而不需要任何自訂宣告。

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  