---
title: 自訂宣告要在與 AD FS 2016 使用 OpenID Connect 或 OAuth 時的 id_token 中發出或更新版本
description: 在 AD FS 2016 或更新版本中的自訂 id k 概念技術概觀
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 04573aa13689a0e6744b01a0fbf8b11b622b2706
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445476"
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016-or-later"></a>自訂宣告要在與 AD FS 2016 使用 OpenID Connect 或 OAuth 時的 id_token 中發出或更新版本

## <a name="overview"></a>總覽
發行項[此處](native-client-with-ad-fs.md)示範如何建置使用 AD FS，如 OpenID Connect 登入的應用程式。 不過，根據預設在只有一組固定的 id_token 中可用的宣告。 AD FS 2016 和更新版本有自訂 id_token OpenID Connect 案例中的功能。

## <a name="when-are-custom-id-token-used"></a>當為自訂的 ID 語彙基元，用嗎？
在某些情況下就可以用戶端應用程式沒有它正在嘗試存取的資源。 因此，它實際上不需要存取權杖。 在這種情況下，用戶端應用程式基本上需要只識別碼權杖，但有一些額外的宣告，以協助功能。

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>取得識別碼的自訂宣告權杖的限制有哪些？

### <a name="scenario-1"></a>案例 1

![限制](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode 會設定為 form_post
2.  只有公用用戶端可以取得自訂宣告在識別碼權杖
3.  信賴憑證者的合作對象識別項 （Web API 識別碼） 應該是相同用戶端識別碼

### <a name="scenario-2"></a>案例 2

![限制](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

具有[KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) AD FS 伺服器上安裝
1.  response_mode 會設定為 form_post
2.  公用和機密用戶端可以取得自訂宣告在識別碼權杖
3.  將範圍 allatclaims 指派至用戶端 – RP 組。
若要將指派範圍，可以使用 Grant ADFSApplicationPermission cmdlet，如下列範例所示：

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-and-configuring-an-oauth-application-to-handle-custom-claims-in-id-token"></a>建立及設定 OAuth 應用程式的識別碼權杖中的自訂宣告
請遵循下列步驟來建立和設定 AD FS 中的應用程式，來接收識別碼權杖和自訂宣告。

### <a name="create-and-configure-an-application-group-in-ad-fs-2016-or-later"></a>建立和設定 AD FS 2016 或更新版本中的應用程式群組

1. 在 AD FS 管理中，以滑鼠右鍵按一下應用程式群組，然後選取**加入應用程式群組**。

2. 在 應用程式群組精靈 中，針對名稱輸入**ADFSSSO**然後在 用戶端-伺服器應用程式選取**存取的 web 應用程式的原生應用程式**範本。 按一下 [下一步]  。

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

3. 複製**用戶端識別元**值。  它會用於稍後做為值的應用程式的 web.config 檔案中的 ida: ClientId。

4. 輸入下列**重新導向 URI:**  -  **https://localhost:44320/** 。  按一下 **\[新增\]** 。 按一下 [下一步]  。

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

5. 在 **設定 Web API**畫面上，輸入下列**識別項** -  **https://contoso.com/WebApp** 。  按一下 **\[新增\]** 。 按一下 [下一步]  。  這個值會用於稍後**ida: ResourceID**應用程式 web.config 檔案中。

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

6. 在 [**選擇存取控制原則**畫面上，選取**允許所有人**然後按一下**下一步]** 。

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

7. 上**設定應用程式權限**畫面上，確定**openid**並**allatclaims**已選取，然後按一下**下一步**。

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap5.png)

8. 在 [**摘要**畫面上，按一下**下一步]** 。  

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap6.png)

9. 在  **Complete**畫面上，按一下**關閉**。

10. 在 AD FS 管理] 中，按一下 [應用程式群組，以取得所有的應用程式群組的清單。 以滑鼠右鍵按一下**ADFSSSO** ，然後選取**屬性**。 選取  **ADFSSSO-Web API** ，按一下 **編輯...**

    ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap7.png)

11. 在  **ADFSSSO-Web API 屬性**畫面上，選取**發佈轉換規則**索引標籤，然後按一下 **新增規則...**

    ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap8.png)

12. 在 **新增轉換宣告規則精靈**畫面上，選取**使用自訂規則傳送宣告**從下拉式清單按一下**下一步**

    ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap9.png)

13. 在上**新增轉換宣告規則精靈**畫面上，輸入**ForCustomIDToken**中**宣告規則名稱**下列宣告中的規則和**自訂規則**. 按一下 **完成**

    ```  
    x:[]
    => issue(claim=x);  
    ```

    ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap10.png)

```

>[!NOTE]
>You can also use PowerShell to assign the allatclaims and openid scopes
>``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "[Client ID from #3 above]" -ServerRoleIdentifier "[Identifier from #5 above]" -ScopeNames "allatclaims","openid"
```

### <a name="download-and-modify-the-sample-application-to-emit-custom-claims-in-idtoken"></a>下載並修改的範例應用程式，以發出在 id_token 中的自訂宣告

本節討論如何下載範例 Web 應用程式，並在 Visual Studio 中修改它。   我們將使用 Azure AD 範例所[此處](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)。  

若要下載範例專案，使用 Git Bash，並輸入下列命令：  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_1.PNG)

#### <a name="to-modify-the-app"></a>若要修改應用程式

1.  開啟使用 Visual Studio 範例。  

2.  因此，所有遺漏的 Nuget 還原，請重建應用程式。  

3.  開啟 web.config 檔案。  讓尋找，如下所示，請修改下列值：  

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

    -   調整 OpenId Connect 中介軟體的初始化邏輯以下列變更：  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];
        private static string resourceId = ConfigurationManager.AppSettings["ida:ResourceID"];
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

    -   註解下列：  

            ```  
            //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
            ```

          ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_3.PNG)

    -   進一步向下，修改的 OpenId Connect 中介軟體選項，如下所示：  

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

    -   更新 about （） 方法，如下所示：  

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

### <a name="test-the-custom-claims-in-id-token"></a>在識別碼權杖中測試自訂宣告

一旦已進行上述變更，請按 f5 鍵。 這會顯示 [範例] 頁面。 按一下 登入。

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_6.PNG)

您將會重新導向到 AD FS 登入頁面。 請繼續進行並登入。

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_7.PNG)

這項作業成功之後，您應該看到您現在登入。

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_8.PNG)

按一下 [關於] 連結。 您會看到 Hello [Username] 會從識別碼權杖中的使用者名稱宣告

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_9.PNG)

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
