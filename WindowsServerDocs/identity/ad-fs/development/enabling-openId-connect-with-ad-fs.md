---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: 建置 web 應用程式使用 OpenID Connect 與 AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74a493e6568b71a05116140ec67586d36f439aa8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882659"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016"></a>建置 web 應用程式使用 OpenID Connect 與 AD FS 2016

>適用於：Windows Server 2016

在 Windows Server 2012 R2 中的 AD FS 中的初始 Oauth 支援建置，AD FS 2016 導入了使用 OpenId Connect 登入的支援。  
  
## <a name="pre-requisites"></a>必要條件  
以下是所需完成這份文件之前的必要元件的清單。 本文件假設已安裝 AD FS，並已建立的 AD FS 伺服器陣列。  
  
-   GitHub 用戶端工具  
  
-   在 Windows Server 2016 TP4 或更新版本的 AD FS  
  
-   Visual Studio 2013 或更新版本。  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>在 AD FS 2016 中建立應用程式群組  
下一節會說明如何在 AD FS 2016 中設定應用程式群組。  
  
#### <a name="create-application-group"></a>建立應用程式群組  
  
1.  在 AD FS 管理中，以滑鼠右鍵按一下應用程式群組，然後選取**加入應用程式群組**。  
  
2.  在 [應用程式群組精靈] 中，針對名稱輸入**ADFSSSO**下方，並在**獨立應用程式**選取**伺服器應用程式或網站**範本。  按一下 [下一步] 。  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  
  
3.  複製**用戶端識別元**值。  它會用於稍後做為值的應用程式的 web.config 檔案中的 ida: ClientId。  
  
4.  輸入下列**重新導向 URI:** - **https://localhost:44320/**。  按一下 **\[新增\]**。 按一下 [下一步] 。  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  
  
5.  在上**設定應用程式認證**畫面上，勾選**產生共用祕密**並複製祕密。 按 [下一步]  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)  
  
6.  在 [**摘要**畫面上，按一下**下一步]**。  
  
7.  在  **Complete**畫面上，按一下**關閉**。  
  
8.  現在，在新的應用程式群組上按一下滑鼠右鍵，然後選取**屬性**。  
  
9. 在  **ADFSSSO 屬性**按一下 **新增應用程式**。  
  
10. 在 **加入新的應用程式範例應用程式**選取**Web API** ，按一下 **下一步**。  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_4.PNG)  
  
11. 在 **設定 Web API**畫面上，輸入下列**識別項** - **https://contoso.com/WebApp**。  按一下 **\[新增\]**。 按一下 [下一步] 。  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG) 
    
12. 在 [**選擇存取控制原則**畫面上，選取**允許所有人**然後按一下**下一步]**。  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. 在 **設定應用程式權限**畫面上，確定**openid**已選取，然後按一下 **下一步**。  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
14. 在 [**摘要**畫面上，按一下**下一步]**。  
  
15. 在  **Complete**畫面上，按一下**關閉**。  
  
16. 在 **範例應用程式屬性**按一下 **確定**。  
  
## <a name="download-and-modify-mvp-app-to-authenticate-via-openid-connect-and-ad-fs"></a>下載並修改 MVC 應用程式，來驗證透過 OpenId Connect 和 AD FS  
本節討論如何下載之範例 Web API，並在 Visual Studio 中修改它。   我們將使用 Azure AD 範例所[此處](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)。  
  
若要下載範例專案，使用 Git Bash，並輸入下列命令：  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  
  
#### <a name="to-modify-the-app"></a>若要修改應用程式  
  
1.  開啟使用 Visual Studio 範例。  
  
2.  因此，所有遺漏的 Nuget 還原，請編譯應用程式。  
  
3.  開啟 web.config 檔案。  讓尋找，如下所示，請修改下列值：  
  
    ```  
    <add key="ida:ClientId" value="8219ab4a-df10-4fbd-b95a-8b53c1d8669e" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://adfs.contoso.com/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:ResourceID" value="https://contoso.com/WebApp"  
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="https://localhost:44320/" />  
    ```  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  
  
4.  開啟 Startup.Auth.cs 檔案，並進行下列變更：  
  
    -   註解下列：  
  
        ```  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
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
  
    -   往下，修改的 OpenId Connect 中介軟體選項，如下所示：  
  
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

