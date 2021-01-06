---
description: 深入瞭解：使用 OAuth 機密用戶端搭配 AD FS 2016 或更新版本建立伺服器端應用程式
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: 使用 OAuth 機密用戶端（AD FS 2016 或更新版本）建立伺服器端應用程式
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.openlocfilehash: fbf316e397d5d3423b046ed6a6b665555ab0cb18
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946394"
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016-or-later"></a>使用 OAuth 機密用戶端（AD FS 2016 或更新版本）建立伺服器端應用程式


AD FS 2016 和更新版本支援可維護其專屬秘密的用戶端，例如在 web 伺服器上執行的應用程式或服務。  這些用戶端稱為機密用戶端。
以下是在 web 伺服器上執行的 web 應用程式，並做為要 AD FS 的機密用戶端的示意圖：

## <a name="pre-requisites"></a>必要條件
以下是完成這份檔之前所需的必要條件清單。 本檔假設已安裝 AD FS。

-   GitHub 用戶端工具

-   Windows Server 2016 TP4 或更新版本中的 AD FS

-   Visual Studio 2013 或更新版本。

## <a name="create-an-application-group-in-ad-fs-2016-or-later"></a>在 AD FS 2016 或更新版本中建立應用程式群組
下一節描述如何在 AD FS 2016 或更新版本中設定應用程式群組。

#### <a name="create-the-application-group"></a>建立應用程式群組

1.  在 AD FS 管理] 中，以滑鼠右鍵按一下 [應用程式群組]，然後選取 [ **新增應用程式群組**]。

2.  在 [應用程式群組] 嚮導中，針對 [ **名稱** ] 輸入 **ADFSOAUTHCC** ，然後在 [ **用戶端-伺服器應用程式** ] 下，選取 **存取 Web API 範本的伺服器應用程式** 。  按一下 [下一步] 。

    ![顯示要在哪裡選取存取 Web API 之伺服器應用程式的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)

3.  複製 **用戶端識別碼** 值。  稍後將用來作為應用程式 web.config 檔中 **ida： ClientId** 的值。

    ![顯示可從中複製用戶端識別碼值的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)

4.  針對 [重新導向 URI] 輸入下列 **內容：**  -  **https://localhost:44323** 。  按一下 [新增] 。 按一下 [下一步] 。

5.  在 [ **設定應用程式認證** ] 畫面上，勾選 [ **產生共用密碼** ] 並複製秘密。  稍後將用來作為應用程式 web.config 檔中 **ida： ClientSecret** 的值。  按一下 [下一步] 。

    ![顯示 [設定應用程式認證] 畫面的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)

6. 在 [**設定 WEB API** ] 畫面上，針對 [**識別碼**] 輸入下列各項  -  **https://contoso.com/WebApp** 。  按一下 [新增] 。 按一下 [下一步] 。  稍後在應用程式 web.config 檔中，將會使用此值來進行 **ida： GraphResourceId** 。

    ![顯示 [設定 Web API] 畫面的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)

7. 在 [套用 **存取控制原則**] 畫面上，選取 [**允許所有人**]，然後按 **[下一步**

    ![顯示 [套用存取控制原則] 畫面的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)

8. 在 [ **設定應用程式許可權** ] 畫面上，確定已選取 **openid** 和 **User_impersonation** ，然後按 **[下一步]**。

    ![顯示 [設定應用程式許可權] 畫面的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)

9. 在 [ **摘要** ] 畫面上，按 **[下一步]**。

10. 在 [ **完成** ] 畫面上，按一下 [ **關閉**]。

## <a name="upgrade-the-database"></a>升級資料庫
建立此逐步解說時使用了 Visual Studio 2015。   為了取得使用 Visual Studio 2015 的範例，您必須更新資料庫檔案。  使用下列程序來執行此動作。

本節討論如何下載範例 Web API，並在 Visual Studio 2015 中升級資料庫。   我們將使用 [此處](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity)的 Azure AD 範例。

若要下載範例專案，請使用 Git Bash，然後輸入下列內容：

```
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git
```

![顯示如何下載範例專案的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)

#### <a name="to-upgrade-the-database-file"></a>升級資料庫檔案

1.  在 Visual Studio 中開啟專案，將會出現一個快顯視窗，告訴您應用程式需要 SQL Server 2012 Express，否則您將需要升級資料庫。  按一下 [確定]。

    ![顯示訊息的螢幕擷取畫面，告訴您應用程式需要 SQL Server 2012 Express，否則您將需要升級資料庫。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)

2.  接著，選取頂端的 [組建 > 組建方案]，以編譯應用程式。  這會還原所有 NuGet 套件。

    ![顯示已成功還原 NuGet 套件的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)

3.  現在在頂端選取 [ **View**  ->  **伺服器總管**]。  一旦開啟，請在 [ **資料連線**] 底下，以滑鼠右鍵按一下 [ **DefaultConnection** ]，然後選取 [ **修改連接**]。

    ![醒目顯示 [修改連接] 功能表選項的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)

4.  在 [ **修改** 連線] 的 [ **資料庫檔案名] ([新增] 或 [現有)**] 底下，選取 **[流覽** 並提供 **path\filename.mdf**]。 在對話方塊中按一下 [ **是]** 。

    ![顯示用來建立資料庫檔案之對話方塊的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)

5.  在 [ **修改連接**] 上，選取 [ **Advanced**]。

    ![顯示 [Advanced] 按鈕的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)

6.  在 [Advanced 屬性] 上，找出 [資料來源]，然後使用下拉式清單將它從 **(LocalDb\v11.0)** 變更為 **(LocalDb) \mssqllocaldb**。

    ![醒目顯示 [資料來源] 欄位的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)

7.  按一下 [確定]。 按一下 [確定]。  按一下 [是] 升級資料庫。

    ![顯示用於升級資料庫之對話方塊的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)

8.  當此動作完成時，請將 [**連接字串**] 旁方塊中的值複製到右側。

    ![顯示 [連接字串] 欄位的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)

9.  現在，開啟 Web.config 檔案，並將 connectionString 中的值取代為您先前複製的值。  儲存 Web.config 檔案。

    > [!NOTE]
    > 需要上述步驟，才能取得新的 connectionString。  否則，當我們執行以下 Update-Database 時，將會發生錯誤。

    ![顯示要在哪裡尋找連接字串值的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)

10. 在 Visual Studio 的頂端，選取 [ **View**  ->  **Other Windows**  ->  **封裝管理員主控台**]。

    ![醒目顯示封裝管理員主控台功能表選項的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)

11. 在底部的封裝管理員主控台中輸入：  `Enable-Migrations` ，然後按 enter 鍵。

    > [!NOTE]
    > 如果您收到錯誤，指出 Enable-Migrations 無法辨識為 Cmdlet，請輸入 Install-Package EntityFramework 來更新 EntityFramework。

    ![Sceenshot，顯示要在哪裡進入啟用遷移。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)

12. 在底部的封裝管理員主控台中輸入：  `Add-Migration <anynamehere>` ，然後按 enter 鍵。

    ![顯示 Add-Migration 測試的輸入位置螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)

13. 在底部的封裝管理員主控台中輸入：  `Update-Database` ，然後按 enter 鍵。

    ![顯示要在哪裡輸入更新資料庫的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)

## <a name="modify-the-webapi-in-visual-studio"></a>修改 Visual Studio 中的 WebApi

#### <a name="to-modify-the-sample-web-api"></a>修改範例 Web API

1.  使用 Visual Studio 開啟範例。

2.  開啟 web.config 檔案。  修改下列值：

    -   ida： ClientId-輸入上述的 [建立應用程式群組] 區段中 #3 的值。

    -   ida： ClientSecret-在 [建立應用程式群組] 區段中輸入 #5 的值。

    -   ida： GraphResourceId-在 [建立應用程式群組] 區段中輸入 #6 的值。

    ![醒目顯示您應該變更之值的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)

3.  在 App_Start 中開啟 Startup.Auth.cs 檔案，並進行下列變更：

    -   註解化下列幾行：

        ```
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);
        ```

    -   在其位置新增下列內容：

        ```
        public static readonly string Authority = "https://<your_fsname>/adfs";
        ```

        其中 <your_fsname> 會取代為 federation service url 的 DNS 部分，例如 adfs.contoso.com

        ![顯示啟動點驗證點 C S 檔案中變更的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)

4.  開啟 UserProfileController.cs 檔案，並進行下列變更：

    -   將下列內容批註：

        ```
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));
        ```

    -   以下列內容取代這兩個專案：

        ```
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));
        ```

        ![顯示使用者設定檔控制器點 C S 檔案變更的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)

    -   將下列內容批註：

        ```
        //authContext = new AuthenticationContext(Startup.Authority);
        ```

    -   以下列內容取代這兩個專案：

        ```
        authContext = new AuthenticationContext(Startup.Authority, false);
        ```

        ![醒目顯示對 >aquiretoken 值所做之變更的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)

    -   現在將下列各項的所有實例標記為批註：

        ```
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString() + "/OAuth");
        ```

    -   將所有出現的專案取代為下列內容：

        ```
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString());
        ```

        ![醒目顯示 [我要重新導向] 的螢幕擷取畫面是我的價值。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)

## <a name="test-the-solution"></a>測試方案
在本節中，我們將測試機密用戶端解決方案。  使用下列程式來測試解決方案。

#### <a name="testing-the-confidential-client-solution"></a>測試機密用戶端解決方案

1. 在 Visual Studio 的頂端，確定已選取 Internet Explorer，然後按一下綠色箭號。

   ![醒目顯示 [Internet Explorer] 按鈕的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)

2. 一旦出現 ASP.Net 頁面，請按一下頁面右上方的 [ **註冊** ]。  輸入使用者名稱和密碼，然後按一下 [ **註冊** ] 按鈕。  這會在 SQL 資料庫中建立本機帳戶。

   ![顯示如何在 S Q L 資料庫中建立本機帳戶的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)

3. 請注意，ASP.NET 網站會顯示 Hello abby@contoso.com ！。  按一下 [設定檔]。

   ![醒目顯示設定檔的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)

4. 這會開啟沒有任何資訊的頁面，並指出我們必須按一下這裡才能登入。  請按一下 **這裡**。

   ![顯示 [使用者設定檔] 頁面的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)

5. 系統現在會提示您登入 AD FS。

   ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)
