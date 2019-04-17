---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: "組建伺服器端的應用程式，使用 AD FS 2016 中的 OAuth 機密戶端"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 175c683f9097aeba4c1f06e8671183476c98aa3f
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016"></a>組建伺服器端的應用程式，使用 AD FS 2016 中的 OAuth 機密戶端

>適用於：Windows Server 2016

在的初始 Oauth 支援 AD FS 在 Windows Server 2012 R2 上建置，AD FS 2016 導入工作維護他們自己的密碼，例如 app 或網頁伺服器上執行之服務的支援。  這些戶端稱為機密戶端。    
以下是圖解網頁伺服器上執行，並且做為來 AD FS 機密 client web 應用程式：  
  
## <a name="pre-requisites"></a>必要條件  
以下是清單之前完成這份文件所需的必要條件。 本文件假設 AD FS 已經安裝，且已建立 AD FS 發電廠。  
  
-   Azure AD 裝機費（免費試用版很好）  
  
-   GitHub client 工具  
  
-   AD FS 在 Windows Server 2016 TP4 或更新版本  
  
-   Visual Studio 2013 或更新版本。  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>AD FS 2016 中建立應用程式群組  
下一節告訴您如何設定 AD FS 2016 中的應用程式群組。  
  
#### <a name="create-the-application-group"></a>建立群組應用程式  
  
1.  AD FS 管理，以滑鼠右鍵按一下應用程式群組，然後選取**[新增應用程式群組**。  
  
2.  在應用程式群組精靈，做為名稱輸入**ADFSOAUTHCC**，在**獨立應用程式**選取**伺服器應用程式或網站**範本。  按一下**下一步**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  
  
3.  複製**Client 識別碼**值。  值為稍後將會使用**ida: ClientId**中的應用程式 web.config 檔案。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  
  
4.  輸入下列項目適用於**重新導向 URI:** - **https://localhost:44323**。  按一下**新增**。 按一下**下一步**。  
  
5.  在**設定應用程式認證**畫面中，將在檢查**產生共用的密碼**和複製密碼。  稍後的值為這將會使用**ida: AppKey**中的應用程式 web.config 檔案。  按一下**下一步**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)  
  
6.  在**摘要**畫面中，按**下**。  
  
7.  在**完成**畫面中，按**關閉**。  
  
8.  現在，以滑鼠右鍵按一下新的應用程式群組上，選取**屬性**。  
  
9. 在**ADFSOAUTHCC 屬性**按**將應用程式新增**。  
  
10. 在**新增新的應用程式範例的應用程式**選取**Web API**並按**下一步**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)  
  
11. 在**設定 Web API**畫面中，輸入下列**識別碼** - **https://contoso.com/WebApp**。  按一下**新增**。 按一下**下一步**。  這個值稍後適用於**ida: GraphResourceId**中的應用程式 web.config 檔案。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  
  
12. 在**選擇存取控制原則**畫面上，選取**允許所有人**按**下一步**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. 在**設定應用程式權限**畫面上，請確定**user_impersonation**選取時，按一下 [**下一步**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  
  
14. 在**摘要**畫面中，按**下**。  
  
15. 在**完成**畫面中，按**關閉**。  
  
16. 在**ADFSOAUTHCC 屬性**按**[確定]**。  
  
## <a name="upgrade-the-database"></a>升級資料庫  
建立本節中使用 visual Studio 2015。   為了取得使用 Visual Studio 2015 範例您會需要更新資料庫檔案。  若要這樣做，使用下列程序。  
  
本節如何下載 Web API 的範例，以及升級資料庫中 Visual Studio 2015。   我們會使用 Azure AD 範例的[在此](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity)。  
  
下載範例專案，使用給 Bash 並輸入下列命令：  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  
  
![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  
  
#### <a name="to-upgrade-the-database-file"></a>若要升級資料庫檔案  
  
1.  Visual Studio 中開放專案、會告知您的應用程式需要 SQL Server 2102 快速快顯，或您將需要升級資料庫。  按一下 \ [確定 \]。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  
  
2.  下一步編譯選取建置的應用程式-> 組建方案，在最上方。  這將會還原所有 NuGet 套件。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  
  
3.  現在在最上方，選取 [**檢視** -> **伺服器總管]**。  一旦開啟，在**資料連接**，以滑鼠右鍵按一下**DefaultConnection**，然後選取**修改連接**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  
  
4.  在**修改連接**、**進階]**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  
  
5.  在 [進階] 功能表上找資料來源，使用下拉式變更從**(LocalDb\v11.0)**以**(LoaclDB) MSSQLLocalDB**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  
  
6.  按一下 \ [確定 \]。 按一下 \ [確定 \]。  按一下 [是] 升級資料庫。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  
  
7.  這完成時，超過右邊，複製值方塊中旁邊**字串連接。**  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  
  
8.  現在，開放 web.config 並取代您複製上方的值與連接字串的值。  儲存 Web.config 檔案。  
  
    > [!NOTE]  
    > 上述步驟是必要的因此我們可以取得新連接字串。  否則，當我們執行 Update-Database 下方會出錯誤。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  
  
9. 在 Visual Studio 的最上方，選取 [**檢視** -> **Windows 其他** -> **封裝管理員」主控台**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  
  
10. 在下方，套件 Manager 主控台中輸入：`Enable-Migrations`和點擊輸入。  
  
    > [!NOTE]  
    > 如果您收到錯誤，指出 Enable-Migrations 無法辨識為 cmdlet，請輸入 Install-Package EntityFramework 更新 EntityFramework。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  
  
11. 在下方，套件 Manager 主控台中輸入：`Add-Migration <anynamehere>`和點擊輸入。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  
  
12. 在下方，套件 Manager 主控台中輸入：`Update-Database`和點擊輸入。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  
  
## <a name="modify-the-webapi-in-visual-studio"></a>修改 Visual Studio 中 WebApi  
  
#### <a name="to-modify-the-sample-web-api"></a>修改範例 Web API  
  
1.  打開使用 Visual Studio 的範例。  
  
2.  打開 web.config。  修改下列值：  
  
    -   ida: ClientId-從 #3 上述輸入值。  
  
    -   ida: AppKey-# 5 上述輸入值。  
  
    -   ida: GraphResourceId-# 11 上述輸入值。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  
  
3.  打開 App_Start Startup.Auth.cs 檔案，並進行下列變更：  
  
    -   查看下列行加：  
  
        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  
  
    -   新增下列該位置：  
  
        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  
  
        < your_fsname > 取代您同盟服務 url，例如 adfs.contoso.com DNS 部分的位置  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_26.PNG)  
  
4.  打開 UserProfileController.cs 檔案，並進行下列變更：  
  
    -   加查看下列動作：  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  
  
    -   這兩個項目取代下列動作：  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  
  
    -   加查看下列動作：  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  
  
    -   這兩個項目取代下列動作：  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  
  
    -   查看所有的執行個體的動作現在意見：  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString() + "/OAuth");  
        ```  
  
    -   使用下列取代所有項目：  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString());  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  
  
## <a name="test-the-solution"></a>測試方案  
在本區段中，我們將測試機密 client 方案。  使用下列程序測試方案。  
  
#### <a name="testing-the-confidential-client-solution"></a>測試機密 client 方案  
  
1.  在 Visual Studio 頂端，請確定已選取 [Internet Explorer，按一下遺漏箭頭。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  
  
2.  一旦 ASP.Net 頁面上出現時，按一下**登記**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
3.  輸入使用者名稱和密碼，然後按一下**登記**。  這會建立本機 account SQL 資料庫。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
4.  請注意，ASP.NET 網站標示為 Hello bsimon。  按一下**設定檔**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  
  
5.  這就會出現的任何資訊頁面，並顯示，我們必須按一下此處以登入。  按一下**在此**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  
  
6.  您現在將會提示登入 AD FS。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  
  
## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  

