---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: "使用 OpenID 連接 AD FS 2016 web 應用程式的組建"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f8040b19576ac9de4ced43e6313cad69276a3d27
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016"></a>使用 OpenID 連接 AD FS 2016 web 應用程式的組建

>適用於：Windows Server 2016

在的初始 Oauth 支援 AD FS 在 Windows Server 2012 R2 上建置，AD FS 2016 導入使用 OpenId 連接登入的支援。  
  
## <a name="pre-requisites"></a>必要條件  
以下是清單之前完成這份文件所需的必要條件。 本文件假設 AD FS 已經安裝，且已建立 AD FS 發電廠。  
  
-   Azure AD 裝機費（免費試用版很好）  
  
-   GitHub client 工具  
  
-   AD FS 在 Windows Server 2016 TP4 或更新版本  
  
-   Visual Studio 2013 或更新版本。  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>AD FS 2016 中建立應用程式群組  
下一節告訴您如何設定 AD FS 2016 中的應用程式群組。  
  
#### <a name="create-application-group"></a>建立群組應用程式  
  
1.  AD FS 管理，以滑鼠右鍵按一下應用程式群組，然後選取**[新增應用程式群組**。  
  
2.  在應用程式群組精靈，做為名稱輸入**ADFSSSO**，在**獨立應用程式**選取**伺服器應用程式或網站**範本。  按一下**下一步**。  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  
  
3.  複製**Client 識別碼**值。  它會作為稍後值 ida: ClientId 應用程式 web.config 檔案。  
  
4.  輸入下列項目適用於**重新導向 URI:** - **https://localhost:44320/**。  按一下**新增**。 按一下**下一步**。  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  
  
5.  在**設定應用程式認證**畫面中，將在檢查**產生共用的密碼**和複製密碼。 按一下**下一步**  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)  
  
6.  在**摘要**畫面中，按**下**。  
  
7.  在**完成**畫面中，按**關閉**。  
  
8.  現在，以滑鼠右鍵按一下新的應用程式群組上，選取**屬性**。  
  
9. 在**ADFSSSO 屬性**按**將應用程式新增**。  
  
10. 在**新增新的應用程式範例的應用程式**選取**Web API**並按**下一步**。  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_4.PNG)  
  
11. 在**設定 Web API**畫面中，輸入下列**識別碼** - **https://contoso.com/WebApp**。  按一下**新增**。 按一下**下一步**。  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
12. 在**選擇存取控制原則**畫面上，選取**允許所有人**按**下一步**。  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. 在**設定應用程式權限**畫面上，請確定**openid**選取時，按一下 [**下一步**。  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
14. 在**摘要**畫面中，按**下**。  
  
15. 在**完成**畫面中，按**關閉**。  
  
16. 在**範例應用程式屬性**按**[確定]**。  
  
## <a name="download-and-modify-mvp-app-to-authenticate-via-openid-connect-and-ad-fs"></a>下載並修改 MVP 透過 OpenId 驗證的應用程式連接和 AD FS  
本節如何下載範例 Web API 與 Visual Studio 中進行修改。   我們會使用 Azure AD 範例的[在此](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)。  
  
下載範例專案，使用給 Bash 並輸入下列命令：  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  
  
#### <a name="to-modify-the-app"></a>修改應用程式  
  
1.  打開使用 Visual Studio 的範例。  
  
2.  編譯應用程式，以便所有遺失 NuGets 還原。  
  
3.  打開 web.config。  修改下列值，以便看起來動作：  
  
    ```  
    <add key="ida:ClientId" value="8219ab4a-df10-4fbd-b95a-8b53c1d8669e" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://adfs.contoso.com/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:ResourceID" value="https://contoso.com/WebApp"  
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="https://localhost:44320/" />  
    ```  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  
  
4.  打開 Startup.Auth.cs 檔案，並進行下列變更：  
  
    -   加查看下列動作：  
  
        ```  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
    -   調整 OpenId 連接介軟體初始化邏輯包含下列變更：  
  
        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  
  
        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  
  
    -   近向下、修改 OpenId 連接介軟體選項如下：  
  
        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                MetadataAddress = metadataAddress,  
                RedirectUri = postLogoutRedirectUri,  
                PostLogoutRedirectUri = postLogoutRedirectUri 
        ```  
  
        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_11.PNG)  
  
        變更上述我們正在進行下列動作：  
  
        -   而不是使用授權的通訊受信任的發行者的相關資料，我們會指定直接透過 MetadataAddress 探索文件位置  
  
        -   Azure AD 不會執行的 redirect_uri 中要求，但是 ADFS。 因此，我們需要新增以下  
  
## <a name="verify-the-app-is-working"></a>請確認正常運作的應用程式  
一旦上述有變更，按下 F5。  這將會出現的範例頁面。  按一下 [登入。  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  
  
您將會重新導向至 AD FS 登入頁面。  請繼續並登入。  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  
  
這是成功之後您應該會看到您現在登入。  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  
  
## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  

