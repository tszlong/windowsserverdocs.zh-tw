---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: 使用 OpenID Connect 搭配 AD FS 2016 和更新版本來建立 web 應用程式
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9b3d64558c27e7b4bda20b6af27e02d55431c94d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358796"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016-and-later"></a>使用 OpenID Connect 搭配 AD FS 2016 和更新版本來建立 web 應用程式

## <a name="pre-requisites"></a>先決條件  
以下是完成本檔之前所需的先決條件清單。 本檔假設已安裝 AD FS，且已建立 AD FS 伺服器陣列。  

-   GitHub 用戶端工具  

-   Windows Server 2016 TP4 或更新版本中的 AD FS  

-   Visual Studio 2013 或更新版本。  

## <a name="create-an-application-group-in-ad-fs-2016-and-later"></a>在 AD FS 2016 和更新版本中建立應用程式群組
下一節說明如何在 AD FS 2016 和更新版本中設定應用程式群組。  

#### <a name="create-application-group"></a>建立應用程式群組  

1.  在 AD FS 管理 中，以滑鼠右鍵按一下 應用程式群組，然後選取 **新增應用程式群組**。  

2.  在 [應用程式組嚮導] 的 [名稱] 中，輸入**ADFSSSO** ，然後在 [**用戶端-伺服器應用程式**] 下，選取**網頁瀏覽器存取 web 應用程式**範本。  按一下 [下一步]。

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  

3.  複製 [**用戶端識別碼**] 值。  稍後在應用程式的 web.config 檔案中，將會使用它做為 ida： ClientId 的值。  

4.  針對 [重新導向 URI] 輸入下列**內容：**  -  **https://localhost:44320/** 。  按一下 [新增]。 按一下 [下一步]。  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  

5.  在 [**摘要**] 畫面上，按 **[下一步]** 。  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)

6.  在 [**完成**] 畫面上，按一下 [**關閉**]。  

## <a name="download-and-modify-sample-application-to-authenticate-via-openid-connect-and-ad-fs"></a>下載並修改範例應用程式，以透過 OpenID Connect 和 AD FS 進行驗證  
本節討論如何下載範例 Web 應用程式，並在 Visual Studio 中加以修改。   我們將使用[這裡](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)的 Azure AD 範例。  

若要下載範例專案，請使用 Git Bash 並輸入下列內容：  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  

#### <a name="to-modify-the-app"></a>修改應用程式  

1.  使用 Visual Studio 開啟範例。  

2.  重建應用程式，以還原所有遺失的 Nuget。  

3.  開啟 web.config 檔案。  修改下列值，讓看起來如下所示：  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 in above section]" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with Redirect URI from #4 in the above section]" />  
    ```  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  

4.  開啟 Startup.Auth.cs 檔案，並進行下列變更：  

    -   批註下列內容：  

        ```  
        //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  

    -   使用下列變更來調整 OpenId Connect 中介軟體初始化邏輯：  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  

    -   接下來，修改 OpenId Connect 中介軟體選項，如下所示：  

        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                MetadataAddress = metadataAddress,  
                PostLogoutRedirectUri = postLogoutRedirectUri,
                RedirectUri = postLogoutRedirectUri
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_11.PNG)  

        藉由變更上述程式，我們會執行下列動作：  

        -   我們不會使用授權來傳達有關受信任簽發者的資料，而是直接透過 MetadataAddress 指定探索 doc 位置。  

        -   Azure AD 不會強制在要求中出現 redirect_uri，但是 ADFS 會執行。 因此，我們需要將它新增到這裡  

## <a name="verify-the-app-is-working"></a>確認應用程式正在運作  
完成上述變更後，按 F5。  這會顯示範例頁面。  按一下 [登入]。  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  

系統會將您重新導向至 AD FS 登入頁面。  請繼續並登入。  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  

成功之後，您應該會看到您現在已登入。  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
