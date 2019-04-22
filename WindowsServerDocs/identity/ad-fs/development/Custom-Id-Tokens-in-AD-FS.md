---
title: 自訂使用 OpenID Connect 或 OAuth 與 AD FS 2016 時，要在 id_token 中發出的宣告
description: 在 AD FS 2016 中自訂識別碼權杖 conecpts 技術概觀
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 8c8e14f22a4bca5b6d32e841814a58a4b4ddca01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820639"
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016"></a>自訂使用 OpenID Connect 或 OAuth 與 AD FS 2016 時，要在 id_token 中發出的宣告

## <a name="overview"></a>總覽
發行項[此處](enabling-openId-connect-with-ad-fs.md)示範如何建置使用 AD FS，如 OpenID Connect 登入的應用程式。 不過，根據預設在只有一組固定的 id_token 中可用的宣告。 AD FS 2016 有自訂的 id_token 中 OpenID Connect 案例能力。

## <a name="when-are-custom-id-token-used"></a>當為自訂的 ID 語彙基元，用嗎？
在某些情況下就可以用戶端應用程式沒有它正在嘗試存取的資源。 因此，它實際上不需要存取權杖。 在這種情況下，用戶端應用程式基本上需要只識別碼權杖，但有一些額外的宣告，以協助功能。

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>取得識別碼的自訂宣告權杖的限制有哪些？

### <a name="scenario-1"></a>案例 1

![限制](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode 會設定為 form_post
2.  只有公用用戶端可以取得自訂宣告在識別碼權杖
3.  信賴憑證者的合作對象識別碼都應該是相同用戶端識別碼

### <a name="scenario-2"></a>案例 2

![限制](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

具有[KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) AD FS 伺服器上安裝
1.  response_mode 會設定為 form_post
2.  將範圍 allatclaims 指派至用戶端 – RP 組。
若要將指派範圍，可以使用 Grant ADFSApplicationPermission cmdlet，如下列範例所示：

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-an-oauth-application-to-handle-custom-claims-in-id-token"></a>建立 OAuth 應用程式的識別碼權杖中的自訂宣告
使用文件[此處](Enabling-OpenId-Connect-with-AD-FS-2016.md)建立應用程式應用程式使用的 OpenID Connect 的 AD FS 登入。 然後遵循下列步驟來設定 AD FS 中的應用程式，來接收識別碼權杖和自訂宣告。

### <a name="create-the-application-group-in-ad-fs-2016"></a>在 AD FS 2016 中建立應用程式群組

1.  建立新的範本，如下所示，為基礎的應用程式群組稱為 CustomTokenClient。

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

2. 此範本會建立機密用戶端。 記下識別碼，並將傳回的 URI 指定為 VS 專案 SSL URL。

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

3.  在下一個步驟中，選取**產生共用祕密**建立用戶端認證，並複製產生的用戶端認證。

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

4. 按一下 [**下一步]** 繼續完成精靈。

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

### <a name="create-the-relying-party"></a>建立信賴憑證者的合作對象
若要新增自訂宣告，在 [識別碼] 語彙基元，您必須建立其宣告會新增識別碼權杖中的 RP。 使用 [新增信賴憑證者信任精靈] 建立新的信賴憑證者合作對象，如下所示：
 
![信賴憑證者的合作對象](media/Custom-Id-Tokens-in-AD-FS/rpsnap1.png)

建立信賴憑證者的合作對象之後，請以滑鼠右鍵按一下信賴憑證者的合作對象項目，然後選取**編輯宣告發佈原則**新增宣告發行規則。 新增必要的自訂宣告的識別碼權杖，如下所示：

![信賴憑證者的合作對象](media/Custom-Id-Tokens-in-AD-FS/rpsnap2.png)

### <a name="assign-allatclaims-scope-to-the-pair-of-client-and-relying-party"></a>將"allatclaims 」 範圍指派給對用戶端和信賴憑證者的合作對象
使用 PowerShell 在 AD FS 伺服器上，指派新的 allatclaims 範圍，下列範例中所指定 (變更的 clientID 和伺服器：

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "5db77ce4-cedf-4319-85f7-cc230b7022e0" -ServerRoleIdentifier "https://customidrp1/" -ScopeNames "allatclaims","openid"
```

>[!NOTE]
>根據您的應用程式設定中變更 ClientRoleIdentifier 和 ServerRoleIdentifier

## <a name="test-the-custom-claims-in-id-token"></a>在識別碼權杖中測試自訂宣告

然後，使用相同的位元，您一律用來存取宣告的程式碼，您就可以看到將成為 id_token 的一部分的其他宣告。
例如，在.NET MVC 範例應用程式中，開啟其中一個控制器檔案，然後輸入類似的程式碼如下：


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
>請留意，在 Oauth2 要求中需要 resource 參數。
>
>不正確的範例：
>
>**https&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token & 範圍 = openid redirect_uri = https&#58;//myportal/auth & nonce = b3ceb943fc756d927777 client_id = 6db3ec2a-075a-4c 72 9b22 ca7ab60cb4e7& 狀態 = 42c2c156aef47e8d0870 資源 = 6db3ec2a-075a-4c 72 9b22 ca7ab60cb4e7**
>
>好的例子：
>
>**https&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token & 範圍 = openid redirect_uri = https&#58;//myportal/auth & nonce = b3ceb943fc756d927777 client_id = 6db3ec2a-075a-4c 72 9b22 ca7ab60cb4e7& 狀態 = 42c2c156aef47e8d0870 resource = https&#58;//customidrp1/ & response_mode = form_post**
>
>如果資源參數不是您可能會收到錯誤或沒有任何自訂宣告的權杖要求中。

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
