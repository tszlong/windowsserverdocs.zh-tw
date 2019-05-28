---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: 建置 web 應用程式使用 OpenID Connect 與 AD FS 2016 和更新版本
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbd42941f8952fc7f649636d2f3645f941240d49
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190422"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016-and-later"></a>建置 web 應用程式使用 OpenID Connect 與 AD FS 2016 和更新版本

## <a name="pre-requisites"></a>必要條件  
以下是所需完成這份文件之前的必要元件的清單。 本文件假設已安裝 AD FS，並已建立的 AD FS 伺服器陣列。  

-   GitHub 用戶端工具  

-   在 Windows Server 2016 TP4 或更新版本的 AD FS  

-   Visual Studio 2013 或更新版本。  

## <a name="create-an-application-group-in-ad-fs-2016-and-later"></a>2016 和更新版本，請在 AD FS 中建立應用程式群組
下一節會說明如何設定應用程式群組中 AD FS 2016 和更新版本。  

#### <a name="create-application-group"></a>建立應用程式群組  

1.  在 AD FS 管理中，以滑鼠右鍵按一下應用程式群組，然後選取**加入應用程式群組**。  

2.  在 [應用程式群組精靈] 中，針對名稱輸入**ADFSSSO**下方，並在**用戶端-伺服器應用程式**選取**存取 web 應用程式的網頁瀏覽器**範本。  按一下 [下一步]  。

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  

3.  複製**用戶端識別元**值。  它會用於稍後做為值的應用程式的 web.config 檔案中的 ida: ClientId。  

4.  輸入下列**重新導向 URI:**  -  **https://localhost:44320/** 。  按一下 **\[新增\]** 。 按一下 [下一步]  。  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  

5.  在 [**摘要**畫面上，按一下**下一步]** 。  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)

6.  在  **Complete**畫面上，按一下**關閉**。  

## <a name="download-and-modify-sample-application-to-authenticate-via-openid-connect-and-ad-fs"></a>下載並修改透過 OpenID Connect 和 AD FS 進行驗證的範例應用程式  
本節討論如何下載範例 Web 應用程式，並在 Visual Studio 中修改它。   我們將使用 Azure AD 範例所[此處](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)。  

若要下載範例專案，使用 Git Bash，並輸入下列命令：  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  

#### <a name="to-modify-the-app"></a>若要修改應用程式  

1.  開啟使用 Visual Studio 範例。  

2.  因此，所有遺漏的 Nuget 還原，請重建應用程式。  

3.  開啟 web.config 檔案。  讓尋找，如下所示，請修改下列值：  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 in above section]" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with Redirect URI from #4 in the above section]" />  
    ```  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  

4.  開啟 Startup.Auth.cs 檔案，並進行下列變更：  

    -   註解下列：  

        ```  
        //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  

    -   調整 OpenId Connect 中介軟體的初始化邏輯以下列變更：  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  

    -   進一步向下，修改的 OpenId Connect 中介軟體選項，如下所示：  

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

        藉由變更上述我們會執行下列作業：  

        -   而不是使用授權單位通訊的受信任的簽發者的相關資料，我們會指定探索文件位置，直接透過 MetadataAddress  

        -   Azure AD 不會強制使用的 redirect_uri，以在要求中，存在但 ADFS。 因此，我們需要將它新增到此處  

## <a name="verify-the-app-is-working"></a>確認應用程式正常運作  
一旦已進行上述變更，請按 f5 鍵。  這會顯示 [範例] 頁面。  按一下 登入。  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  

您將會重新導向到 AD FS 登入頁面。  請繼續進行並登入。  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  

這項作業成功之後，您應該看到您現在登入。  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
