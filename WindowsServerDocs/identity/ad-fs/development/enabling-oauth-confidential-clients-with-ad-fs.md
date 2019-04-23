---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: 建置使用 OAuth 機密用戶端與 AD FS 2016 的伺服器端應用程式
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 175c683f9097aeba4c1f06e8671183476c98aa3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869579"
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016"></a>建置使用 OAuth 機密用戶端與 AD FS 2016 的伺服器端應用程式

>適用於：Windows Server 2016

在 Windows Server 2012 R2 中的 AD FS 中的初始 Oauth 支援建置，AD FS 2016 導入了支援的用戶端能夠維護其本身的祕密，例如應用程式或 web 伺服器上執行的服務。  這些用戶端稱為機密用戶端。    
以下是 web 應用程式 web 伺服器上執行，並為 AD FS 的機密用戶端提供的圖解：  
  
## <a name="pre-requisites"></a>必要條件  
以下是所需完成這份文件之前的必要元件的清單。 本文件假設已安裝 AD FS，並已建立的 AD FS 伺服器陣列。  
  
-   （免費試用版是沒問題） 的 Azure AD 訂用帳戶  
  
-   GitHub 用戶端工具  
  
-   在 Windows Server 2016 TP4 或更新版本的 AD FS  
  
-   Visual Studio 2013 或更新版本。  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>在 AD FS 2016 中建立應用程式群組  
下一節會說明如何在 AD FS 2016 中設定應用程式群組。  
  
#### <a name="create-the-application-group"></a>建立應用程式群組  
  
1.  在 AD FS 管理中，以滑鼠右鍵按一下應用程式群組，然後選取**加入應用程式群組**。  
  
2.  在 [應用程式群組精靈] 中，針對名稱輸入**ADFSOAUTHCC**下方，並在**獨立應用程式**選取**伺服器應用程式或網站**範本。  按一下 [下一步] 。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  
  
3.  複製**用戶端識別元**值。  它將在稍後用做為值**ida: ClientId**應用程式 web.config 檔案中。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  
  
4.  輸入下列**重新導向 URI:** - **https://localhost:44323**。  按一下 **\[新增\]**。 按一下 [下一步] 。  
  
5.  在上**設定應用程式認證**畫面上，勾選**產生共用祕密**並複製祕密。  這稍後會用做為值**ida: AppKey**應用程式 web.config 檔案中。  按一下 [下一步] 。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)  
  
6.  在 [**摘要**畫面上，按一下**下一步]**。  
  
7.  在  **Complete**畫面上，按一下**關閉**。  
  
8.  現在，在新的應用程式群組上按一下滑鼠右鍵，然後選取**屬性**。  
  
9. 在  **ADFSOAUTHCC 屬性**按一下 **新增應用程式**。  
  
10. 在 **加入新的應用程式範例應用程式**選取**Web API**，按一下 **下一步**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)  
  
11. 在 **設定 Web API**畫面上，輸入下列**識別項** - **https://contoso.com/WebApp**。  按一下 **\[新增\]**。 按一下 [下一步] 。  這個值會用於稍後**ida: GraphResourceId**應用程式 web.config 檔案中。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  
  
12. 在 [**選擇存取控制原則**畫面上，選取**允許所有人**然後按一下**下一步]**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. 在 **設定應用程式權限**畫面上，確定**user_impersonation**已選取，然後按一下 **下一步**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  
  
14. 在 [**摘要**畫面上，按一下**下一步]**。  
  
15. 在  **Complete**畫面上，按一下**關閉**。  
  
16. 在  **ADFSOAUTHCC 屬性**按一下 **確定**。  
  
## <a name="upgrade-the-database"></a>升級資料庫  
Visual Studio 2015 中建立這個逐步解說中使用。   若要取得使用 Visual Studio 2015 的範例，您必須更新資料庫檔案。  請使用下列程序來執行此動作。  
  
本節討論如何下載範例 Web API 和升級 Visual Studio 2015 中的資料庫。   我們將使用 Azure AD 範例所[此處](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity)。  
  
若要下載範例專案，使用 Git Bash，並輸入下列命令：  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  
  
![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  
  
#### <a name="to-upgrade-the-database-file"></a>若要升級的資料庫檔案  
  
1.  在 Visual Studio 中開啟專案，會快顯視窗，告知您的應用程式需要 SQL Server 2102 Express 或您必須升級資料庫。  按一下 [確定]。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  
  
2.  下一步] 編譯應用程式中，選取 [建置-> 建置方案，在頂端。  這會還原所有的 NuGet 套件。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  
  
3.  現在，在頂端，選取**檢視** -> **伺服器總管**。  一旦開啟，底下**資料連接**，以滑鼠右鍵按一下**DefaultConnection** ，然後選取**修改連接**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  
  
4.  在 **修改連接**，選取**進階**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  
  
5.  在 進階屬性中，找到資料來源，並使用下拉式清單來變更從中 **(LocalDb\v11.0)** 要 **(LoaclDB) MSSQLLocalDB**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  
  
6.  按一下 [確定]。 按一下 [確定]。  按一下 [是]，升級資料庫。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  
  
7.  完成此動作時，超過在右側，將值複製到 方塊旁**連接字串。**  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  
  
8.  現在，開啟 Web.config 檔案，並將上面複製的值使用的連接字串中的值。  儲存 Web.config 檔案。  
  
    > [!NOTE]  
    > 上述步驟所需，以便我們能取得新的連接字串。  否則，當我們執行更新資料庫下它就會發生錯誤。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  
  
9. 在 Visual Studio 的頂端，選取**檢視** -> **其他 Windows** -> **Package Manager Console**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  
  
10. 在底部，在 Package Manager Console 輸入：`Enable-Migrations`然後按 enter 鍵。  
  
    > [!NOTE]  
    > 如果您收到錯誤指出 Enable-migrations 無法辨識為 cmdlet 中，輸入 Install-package EntityFramework 更新 EntityFramework。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  
  
11. 在底部，在 Package Manager Console 輸入：`Add-Migration <anynamehere>`然後按 enter 鍵。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  
  
12. 在底部，在 Package Manager Console 輸入：`Update-Database`然後按 enter 鍵。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  
  
## <a name="modify-the-webapi-in-visual-studio"></a>修改在 Visual Studio WebApi  
  
#### <a name="to-modify-the-sample-web-api"></a>若要修改的範例 Web API  
  
1.  開啟使用 Visual Studio 範例。  
  
2.  開啟 web.config 檔案。  修改下列值：  
  
    -   ida: ClientId-上述的 #3 輸入的值。  
  
    -   ida: AppKey-輸入上述的 #5 的值。  
  
    -   ida: GraphResourceId-輸入上述的 #11 的值。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  
  
3.  開啟 App_Start Startup.Auth.cs 檔案，並進行下列變更：  
  
    -   註解下列幾行：  
  
        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  
  
    -   在該位置中加入下列內容：  
  
        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  
  
        其中 < your_fsname > 會取代為您的同盟服務 url，例如 adfs.contoso.com 的 DNS 部分  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_26.PNG)  
  
4.  開啟 UserProfileController.cs 檔案，並進行下列變更：  
  
    -   註解下列：  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  
  
    -   您可以將這兩個項目取代為下列：  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  
  
    -   註解下列：  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  
  
    -   您可以將這兩個項目取代為下列：  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  
  
    -   現在註解化下列的所有執行個體：  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString() + "/OAuth");  
        ```  
  
    -   以下列內容取代所有出現項目：  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString());  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  
  
## <a name="test-the-solution"></a>測試方案  
在本節中，我們將測試機密用戶端解決方案。  您可以使用下列程序來測試方案。  
  
#### <a name="testing-the-confidential-client-solution"></a>機密用戶端解決方案的測試  
  
1.  在 Visual Studio 的頂端，確定已選取 Internet Explorer，然後按一下綠色箭號。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  
  
2.  一旦啟動，ASP.Net 網頁時，按一下**註冊**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
3.  輸入使用者名稱和密碼，然後按一下**註冊**。  這是 SQL database 中建立本機帳戶。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
4.  請注意 ASP.NET 網站現在，顯示 Hello 是 bsimon ！。  按一下 **設定檔**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  
  
5.  這會顯示不含任何資訊的頁面，並指出，我們必須按一下這裡以登入。  按一下 **此處**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  
  
6.  您現在會提示登入 AD FS。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  
  
## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  

