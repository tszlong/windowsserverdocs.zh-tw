---
title: 使用 OpenID Connect 或 OAuth 搭配 AD FS 2016 或更新版本時，自訂要在 id_token 中發出的宣告
description: AD FS 2016 或更新版本中自訂識別碼權杖概念的技術總覽
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 04/29/2020
ms.topic: article
ms.reviewer: anandy
ms.openlocfilehash: bd9ed9d44c18eade653bfea6daa5cf6aa89e06ea
ms.sourcegitcommit: 7674bbe49517bbfe0e2c00160e08240b60329fd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/20/2021
ms.locfileid: "98603451"
---
# <a name="customize-claims-to-be-emitted-in-id_token-when-using-openid-connect-or-oauth-with-ad-fs-2016-or-later"></a>使用 OpenID Connect 或 OAuth 搭配 AD FS 2016 或更新版本時，自訂要在 id_token 中發出的宣告

## <a name="overview"></a>概觀

[本文說明如何](native-client-with-ad-fs.md)建立使用 AD FS 進行 OpenID Connect 登入的應用程式。 不過，根據預設，id_token 中只會提供一組固定的宣告。 AD FS 2016 和更新版本可讓您在 OpenID Connect 案例中自訂 id_token。

## <a name="when-are-custom-id-tokens-used"></a>何時使用自訂識別碼權杖？

在某些情況下，用戶端應用程式可能沒有其嘗試存取的資源。 因此，它其實不需要存取權杖。 在這種情況下，用戶端應用程式基本上只需要識別碼權杖，但是有一些額外的宣告可協助功能。

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>在識別碼權杖中取得自訂宣告有哪些限制？

### <a name="scenario-1"></a>案例 1

![顯示案例1的螢幕擷取畫面，其中使用信賴憑證者 I D 來等於用戶端 D。](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1. `response_mode` 設定為 `form_post`
2. 只有公用用戶端可以取得識別碼權杖中的自訂宣告
3.  (Web API 識別碼) 的信賴憑證者識別碼應與用戶端識別碼相同

### <a name="scenario-2"></a>案例 2

![顯示使用 allatclaims 範圍之案例2的螢幕擷取畫面。](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

在 AD FS 伺服器上安裝 [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) 或更新版本的安全性更新
1. `response_mode` 設定為 form_post
2. 公用和機密用戶端都可以取得識別碼權杖中的自訂宣告
3. 將範圍指派 `allatclaims` 給用戶端– RP 配對。

您可以使用 Cmdlet 來指派範圍， `Grant-ADFSApplicationPermission` 如下列範例所示：

```powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-and-configuring-an-oauth-application-to-handle-custom-claims-in-id-token"></a>建立和設定 OAuth 應用程式以處理識別碼權杖中的自訂宣告

遵循下列步驟，在 AD FS 中建立及設定應用程式，以使用自訂宣告接收識別碼權杖。

### <a name="create-and-configure-an-application-group-in-ad-fs-2016-or-later"></a>在 AD FS 2016 或更新版本中建立和設定應用程式群組

1. 在 AD FS 管理] 中，以滑鼠右鍵按一下 [應用程式群組]，然後選取 [ **新增應用程式群組**]。
2. 在 [應用程式群組] 嚮導中，針對 [名稱] 輸入 **ADFSSSO** ，在 [Client-Server 應用程式] 下，選取 **存取 web 應用程式範本的原生應用程式** 按 [下一步] 。

   ![[新增應用程式群組] Wizard [歡迎使用] 頁面的螢幕擷取畫面。](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

3. 複製 **用戶端識別碼** 值。  稍後將用來作為應用程式 web.config 檔中 ida： ClientId 的值。
4. 針對 [重新導向 URI] 輸入下列 **內容：**  -  **https://localhost:44320/** 。  按一下 [新增] 。 按 [下一步] 。

   ![[新增應用程式群組] Wizard 的 [原生應用程式] 頁面的螢幕擷取畫面，其中顯示重新導向 U R I。](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

5. 在 [**設定 WEB API** ] 畫面上，針對 [**識別碼**] 輸入下列各項  -  **https://contoso.com/WebApp** 。  按一下 [新增] 。 按 [下一步] 。  此值稍後將用於 **ida：** 應用程式 web.config 檔中的 ResourceID。

   ![[新增應用程式群組] 頁面的 [設定 Web API] 頁面的螢幕擷取畫面，其中顯示正確的識別碼。](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

6. 在 [**選擇存取控制原則**] 畫面上，選取 [**允許所有人**]，然後按 **[下一步**

   ![[新增應用程式群組] Wizard 的 [選擇存取控制原則] 頁面的螢幕擷取畫面，其中顯示已醒目提示 [允許每個人] 選項。](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

7. 在 [ **設定應用程式許可權** ] 畫面上，確定已選取 **openid** 和 **Allatclaims** ，然後按 **[下一步]**。

   ![[新增應用程式群組] Wizard 的 [設定應用程式許可權] 頁面的螢幕擷取畫面。](media/Custom-Id-Tokens-in-AD-FS/clientsnap5.PNG)

8. 在 [ **摘要** ] 畫面上，按 **[下一步]**。

   ![[新增應用程式群組] Wizard 的 [摘要] 頁面螢幕擷取畫面。](media/Custom-Id-Tokens-in-AD-FS/clientsnap6.PNG)

9. 在 [ **完成** ] 畫面上，按一下 [ **關閉**]。
10. 在 AD FS 管理] 中，按一下 [應用程式群組] 以取得所有應用程式群組的清單。 以滑鼠右鍵按一下 **ADFSSSO** ，然後選取 [ **屬性**]。 選取 [ **ADFSSSO-WEB API** ]，然後按一下 [**編輯**]。

    ![顯示 [應用程式] 區段中所列之 Web API 的 [D F S O 屬性] 對話方塊螢幕擷取畫面。](media/Custom-Id-Tokens-in-AD-FS/clientsnap7.PNG)

11. 在 [ **ADFSSSO-WEB API 屬性**] 畫面上，選取 [**發行轉換規則**] 索引標籤，然後按一下 [**新增規則 ...** ]。

    ![顯示 [發行轉換規則] 索引標籤的 [D F S O S O 屬性] 對話方塊的螢幕擷取畫面。](media/Custom-Id-Tokens-in-AD-FS/clientsnap8.PNG)

12. 在 [**新增轉換宣告規則]** 畫面上，從下拉式清單中選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]** 。

    ![[新增轉換宣告規則] 的 [選取規則範本] 頁面的螢幕擷取畫面，其中顯示已選取 [使用自訂規則傳送宣告] 選項。](media/Custom-Id-Tokens-in-AD-FS/clientsnap9.PNG)

13. 在 [**新增轉換宣告規則]** 頁面中，于 [宣告 **規則名稱**] 和 [**自訂規則** 中的下列宣告規則] 中輸入 **ForCustomIDToken** 。 按一下 [完成]

    ```
    x:[]
    => issue(claim=x);
    ```

    ![[新增轉換宣告規則] 頁面的 [設定規則] 頁面的螢幕擷取畫面，其中已填入 [宣告規則名稱] 和 [自訂規則文字] 欄位。](media/Custom-Id-Tokens-in-AD-FS/clientsnap10.PNG)

    > [!NOTE]
    > 您也可以使用 PowerShell 來指派 `allatclaims` 和 `openid` 範圍。

    ``` powershell
    Grant-AdfsApplicationPermission -ClientRoleIdentifier "[Client ID from #3 above]" -ServerRoleIdentifier "[Identifier from #5 above]" -ScopeNames "allatclaims","openid"
    ```

### <a name="download-and-modify-the-sample-application-to-emit-custom-claims-in-id_token"></a>下載並修改範例應用程式，以在 id_token 中發出自訂宣告

本節討論如何下載範例 Web 應用程式，並在 Visual Studio 中加以修改。 我們將使用位於 [此處](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC)的 Azure AD 範例。

若要下載範例專案，請使用 Git Bash，然後輸入下列內容：

```
git clone https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC
```

![Git Bash 視窗的螢幕擷取畫面，其中顯示 git clone 命令的結果。](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_1.PNG)

#### <a name="to-modify-the-app"></a>若要修改應用程式

1. 使用 Visual Studio 開啟範例。
2. 重建應用程式，以便還原所有遺失的 NuGet 套件。
3. 開啟 web.config 檔案。  修改下列值，使其看起來如下所示：

   ```xml
   <add key="ida:ClientId" value="[Replace this Client Id from #3 above under section Create and configure an Application Group in AD FS 2016 or later]" />
   <add key="ida:ResourceID" value="[Replace this with the Web API Identifier from #5 above]"  />
   <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />
   <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />
   <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->
   <add key="ida:PostLogoutRedirectUri" value="[Replace this with the Redirect URI from #4 above]" />
   ```

   ![Web 設定檔的螢幕擷取畫面，其中顯示修改過的值。](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_2.png)

4. 開啟 Startup.Auth.cs 檔案，並進行下列變更：

   - 使用下列變更來調整 OpenId Connect 中介軟體初始化邏輯：

      ```cs
      private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
      //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
      //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
      private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];
      private static string resourceId = ConfigurationManager.AppSettings["ida:ResourceID"];
      private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];
      ```

   - 將下列內容批註：

      ```cs
      //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);
      ```

      ![啟動驗證檔案的螢幕擷取畫面，其中顯示已批註的程式程式碼。](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_3.png)

   - 從下面的步驟中，修改 OpenId Connect 中介軟體選項，如下所示：

      ```cs
      app.UseOpenIdConnectAuthentication(
          new OpenIdConnectAuthenticationOptions
          {
              ClientId = clientId,
              //Authority = authority,
              Resource = resourceId,
              MetadataAddress = metadataAddress,
              PostLogoutRedirectUri = postLogoutRedirectUri,
              RedirectUri = postLogoutRedirectUri
      ```

      ![啟動驗證檔案的螢幕擷取畫面，其中顯示已修改的 Open I D Connect 中介軟體選項。](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_4.png)

5. 開啟 HomeController.cs 檔案，並進行下列變更：

   - 新增下列內容：

      ```cs
      using System.Security.Claims;
      ```

   - 更新 `About()` 方法，如下所示：

      ```cs
      [Authorize]
      public ActionResult About()
      {
          ClaimsPrincipal cp = ClaimsPrincipal.Current;
          string userName = cp.FindFirst(ClaimTypes.WindowsAccountName).Value;
          ViewBag.Message = String.Format("Hello {0}!", userName);
          return View();
      }
      ```

      ![具有才能更新之首頁控制器檔案的螢幕擷取畫面。](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_5.png)

### <a name="test-the-custom-claims-in-id-token"></a>測試識別碼權杖中的自訂宣告

完成上述變更之後，按 F5。 這會顯示範例頁面。 按一下 [登入]。

![顯示在瀏覽器中範例應用程式的螢幕擷取畫面。](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_6.PNG)

系統會將您重新導向至 AD FS 登入頁面。 繼續進行並登入。

![D F S 登入頁面的螢幕擷取畫面。](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_7.PNG)

一旦成功，您應該會看到您現在已登入。

![範例應用程式的螢幕擷取畫面，顯示使用者已登入。](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_8.png)

按一下 [關於]  連結。 您會看到 "Hello [Username]"，它是從識別碼權杖中的使用者名稱宣告取出的

![範例應用程式中 [關於] 頁面的螢幕擷取畫面。](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_9.png)

## <a name="next-steps"></a>後續步驟

[AD FS 開發](../../ad-fs/AD-FS-Development.md)
