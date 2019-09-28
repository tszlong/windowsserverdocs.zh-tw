---
title: 在使用 OpenID Connect 或 OAuth 搭配 AD FS 2016 或更新版本時，自訂要在 id_token 中發出的宣告
description: AD FS 2016 或更新版本中自訂識別碼權杖概念的技術總覽
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 88ae6837872c5a6cf6bb1d8533a0aa14b82ca573
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358915"
---
# <a name="customize-claims-to-be-emitted-in-id_token-when-using-openid-connect-or-oauth-with-ad-fs-2016-or-later"></a>在使用 OpenID Connect 或 OAuth 搭配 AD FS 2016 或更新版本時，自訂要在 id_token 中發出的宣告

## <a name="overview"></a>總覽
[本文說明如何](native-client-with-ad-fs.md)建立使用 AD FS 進行 OpenID connect 登入的應用程式。 不過，根據預設，id_token 中只會提供一組固定的宣告。 AD FS 2016 和更新版本可讓您自訂 OpenID Connect 案例中的 id_token。

## <a name="when-are-custom-id-token-used"></a>使用自訂識別碼權杖的時機？
在某些情況下，用戶端應用程式可能不會有其嘗試存取的資源。 因此，它實際上不需要存取權杖。 在這種情況下，用戶端應用程式基本上只需要識別碼權杖，但有一些額外的宣告可協助功能。

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>取得識別碼權杖中的自訂宣告有哪些限制？

### <a name="scenario-1"></a>案例 1

![加以](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode 設定為 form_post
2.  只有公用用戶端可以取得識別碼權杖中的自訂宣告
3.  信賴憑證者識別碼（Web API 識別碼）應與用戶端識別碼相同

### <a name="scenario-2"></a>案例2

![加以](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

將[KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472)安裝在 AD FS 伺服器上
1.  response_mode 設定為 form_post
2.  公用和機密用戶端都可以取得識別碼權杖中的自訂宣告
3.  將範圍 allatclaims 指派給用戶端– RP 配對。
您可以使用 ADFSApplicationPermission Cmdlet 來指派範圍，如下列範例所示：

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-and-configuring-an-oauth-application-to-handle-custom-claims-in-id-token"></a>建立和設定 OAuth 應用程式以處理識別碼權杖中的自訂宣告
請遵循下列步驟，在 AD FS 中建立和設定應用程式，以使用自訂宣告接收識別碼權杖。

### <a name="create-and-configure-an-application-group-in-ad-fs-2016-or-later"></a>在 AD FS 2016 或更新版本中建立和設定應用程式群組

1. 在 AD FS 管理 中，以滑鼠右鍵按一下 應用程式群組，然後選取 **新增應用程式群組**。

2. 在 [應用程式群組] 上，針對 [名稱] 輸入**ADFSSSO** ，然後在 [用戶端-伺服器應用程式] 下，選取**存取 web 應用程式範本的原生應用程式** 按一下 [下一步]。

   ![用戶端](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

3. 複製 [**用戶端識別碼**] 值。  稍後在應用程式的 web.config 檔案中，將會使用它做為 ida： ClientId 的值。

4. 針對 [重新導向 URI] 輸入下列**內容：**  -  **https://localhost:44320/** 。  按一下 [新增]。 按一下 [下一步]。

   ![用戶端](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

5. 在 [**設定 WEB API** ] 畫面上，針對 [**識別碼** -  **https://contoso.com/WebApp** ] 輸入下列各項。  按一下 [新增]。 按一下 [下一步]。  稍後在應用程式的 web.config 檔案中，將會使用此值進行**ida： ResourceID** 。

   ![用戶端](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

6. 在 [**選擇存取控制原則**] 畫面上選取 [**允許每個人**]，然後按 **[下一步]**

   ![用戶端](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

7. 在 [**設定應用程式許可權**] 畫面上，確認已選取**openid**和**Allatclaims** ，然後按 **[下一步]** 。

   ![用戶端](media/Custom-Id-Tokens-in-AD-FS/clientsnap5.png)

8. 在 [**摘要**] 畫面上，按 **[下一步]** 。  

   ![用戶端](media/Custom-Id-Tokens-in-AD-FS/clientsnap6.png)

9. 在 [**完成**] 畫面上，按一下 [**關閉**]。

10. 在 AD FS 管理 中，按一下 應用程式群組 以取得所有應用程式群組的清單。 以滑鼠右鍵按一下 [ **ADFSSSO** ]，然後選取 [**屬性**]。 選取 [ **ADFSSSO-WEB API** ]，然後按一下 [**編輯**]

    ![用戶端](media/Custom-Id-Tokens-in-AD-FS/clientsnap7.png)

11. 在 [ **ADFSSSO-WEB API 屬性**] 畫面上，選取 [**發行轉換規則**] 索引標籤，然後按一下 [**新增規則 ...** ]

    ![用戶端](media/Custom-Id-Tokens-in-AD-FS/clientsnap8.png)

12. 在 [**新增轉換宣告規則嚮導]** 畫面上，從下拉式選單選取 [**使用自訂規則傳送宣告**]，然後按 **[下一步]**

    ![用戶端](media/Custom-Id-Tokens-in-AD-FS/clientsnap9.png)

13. 在 [**新增轉換宣告規則嚮導]** 畫面上，于 [宣告**規則名稱**] 和 [**自訂規則**中的宣告規則] 中輸入**ForCustomIDToken** 。 按一下 **[完成]**

    ```  
    x:[]
    => issue(claim=x);  
    ```

    ![用戶端](media/Custom-Id-Tokens-in-AD-FS/clientsnap10.png)

```

>[!NOTE]
>You can also use PowerShell to assign the allatclaims and openid scopes
>``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "[Client ID from #3 above]" -ServerRoleIdentifier "[Identifier from #5 above]" -ScopeNames "allatclaims","openid"
```

### <a name="download-and-modify-the-sample-application-to-emit-custom-claims-in-id_token"></a>下載並修改範例應用程式，以在 id_token 中發出自訂宣告

本節討論如何下載範例 Web 應用程式，並在 Visual Studio 中加以修改。   我們將使用[這裡](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)的 Azure AD 範例。  

若要下載範例專案，請使用 Git Bash 並輸入下列內容：  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_1.PNG)

#### <a name="to-modify-the-app"></a>修改應用程式

1.  使用 Visual Studio 開啟範例。  

2.  重建應用程式，以還原所有遺失的 Nuget。  

3.  開啟 web.config 檔案。  修改下列值，讓看起來如下所示：  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 above under section Create and configure an Application Group in AD FS 2016 or later]" />  
    <add key="ida:ResourceID" value="[Replace this with the Web API Identifier from #5 above]"  />
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with the Redirect URI from #4 above]" />  
    ```  

    ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_2.PNG)  

4.  開啟 Startup.Auth.cs 檔案，並進行下列變更：  

    -   使用下列變更來調整 OpenId Connect 中介軟體初始化邏輯：  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];
        private static string resourceId = ConfigurationManager.AppSettings["ida:ResourceID"];
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

    -   批註下列內容：  

            ```  
            //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
            ```

          ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_3.PNG)

    -   接下來，修改 OpenId Connect 中介軟體選項，如下所示：  

        ```  
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

        ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_4.PNG)

5.  開啟 HomeController.cs 檔案，並進行下列變更：  

    -   加入下列內容：  

            ```  
            using System.Security.Claims;  
            ```

    -   更新 About （）方法，如下所示：  

        ```  
        [Authorize]
        public ActionResult About()
        {
            ClaimsPrincipal cp = ClaimsPrincipal.Current;
            string userName = cp.FindFirst(ClaimTypes.WindowsAccountName).Value;
            ViewBag.Message = String.Format("Hello {0}!", userName);
            return View();
        }
        ```  

        ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_5.PNG)

### <a name="test-the-custom-claims-in-id-token"></a>測試識別碼權杖中的自訂宣告

完成上述變更後，按 F5。 這會顯示範例頁面。 按一下 [登入]。

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_6.PNG)

系統會將您重新導向至 AD FS 登入頁面。 請繼續並登入。

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_7.PNG)

成功之後，您應該會看到您現在已登入。

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_8.PNG)

按一下 [關於連結]。 您會看到 Hello [Username] 從識別碼權杖中的使用者名稱宣告抓取

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_9.PNG)

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
