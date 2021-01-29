---
description: 建立使用 OAuth 機密用戶端的伺服器端應用程式。 使用 AD FS 2016 或更新版本。
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: 使用 Active Directory 同盟服務2016或更新版本，建立使用 OAuth 機密用戶端的伺服器端應用程式
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.openlocfilehash: 2302d8fd1d6d00943316a62578d1d8d9ee096169
ms.sourcegitcommit: d1815253b47e776fb96a3e91556fd231bef8ee6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2021
ms.locfileid: "99042574"
---
# <a name="build-a-server-side-app-that-uses-oauth-confidential-clients-by-using-ad-fs-2016-or-later"></a>使用 AD FS 2016 或更新版本，建立使用 OAuth 機密用戶端的伺服器端應用程式


Active Directory 同盟服務 (AD FS) 2016 和更新版本支援可維護其專屬秘密的用戶端，例如在 web 伺服器上執行的應用程式或服務。  這些用戶端稱為 *機密客戶* 端。

本文說明在 web 伺服器上執行的 web 應用程式。 應用程式會作為要 AD FS 的機密用戶端。

## <a name="prerequisites"></a>必要條件
您將需要下列資源： 

-   GitHub 用戶端工具。

-   Windows Server 2016 Technical Preview 4 或更新版本中的 AD FS。  (本文假設已安裝 AD FS。 ) 

-   Visual Studio 2013 或更新版本。

## <a name="create-an-application-group"></a>建立應用程式群組

若要在 AD FS 2016 或更新版本中建立應用程式群組：

1.  在 AD FS 管理] 中，以滑鼠右鍵按一下 [ **應用程式群組**]。 然後選取 [ **新增應用程式群組**]。

2.  在 [ **新增應用程式群組] 嚮導** 中： 
    1. 在 [ **名稱**] 底下，輸入 *ADFSOAUTHCC*。 
    1. 在 [ **用戶端-伺服器應用程式**] 下，選取 **存取 Web API 範本的伺服器應用程式** 。  
    1. 選取 [下一步] 。

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG" alt-text="螢幕擷取畫面，顯示要在哪裡選取用來存取 Web A P I 的伺服器應用程式範本。" lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG":::

3.  複製 **用戶端識別碼** 值。 您稍後會在應用程式的 *web.config* 檔中使用它。 它是的值 `ida:ClientId` 。

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG" alt-text="顯示要在哪裡複製用戶端識別碼值的螢幕擷取畫面。" lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG":::

4. 針對 [重新 **導向 URI**]，輸入 *https://localhost:44323* 。  選取 [ **新增**]，然後選取 **[下一步]**。

5.  在 [ **設定應用程式認證** ] 頁面上： 
    1. 選取 [ **產生共用密碼**]。 
    1. 複製秘密。 您稍後會在應用程式的 *web.config* 檔中使用此秘密。 它是的值 `ida:ClientSecret` 。  
    1. 選取 [下一步] 。

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG" alt-text="顯示 [設定應用程式認證] 頁面的螢幕擷取畫面。" lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG":::

6. 在 [ **設定 WEB API** ] 頁面上： 
    1. 針對 [ **識別碼**]，輸入 *https://contoso.com/WebApp* 。 您稍後會在應用程式的 *web.config* 檔中使用此值。 它是的值 `ida:GraphResourceId` 。 
    1. 選取 [新增]。 
    1. 選取 [下一步] 。  

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG" alt-text="顯示 [設定 Web A P I] 頁面的螢幕擷取畫面。" lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG":::

7. 在 [套用 **存取控制原則** ] 頁面上，選取 [ **允許每個人**]。 然後選取 [下一步]。

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG" alt-text="顯示 [套用存取控制原則] 頁面的螢幕擷取畫面。" lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG":::

8. 在 [ **設定應用程式許可權** ] 頁面上，確認已選取 [ **openid** ] 和 [ **user_impersonation** ]。 然後選取 [下一步]。

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG" alt-text="顯示 [設定應用程式許可權] 頁面的螢幕擷取畫面。" lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG":::

9. 在 [ **摘要** ] 頁面上，選取 **[下一步]**。

10. 在 [ **完成** ] 頁面上，選取 [ **關閉**]。

## <a name="upgrade-the-database"></a>升級資料庫
本文是以 Visual Studio 2015 為基礎。 若要讓範例使用 Visual Studio 2015，請遵循本節中的指示來升級資料庫檔案。

本節討論如何下載範例 web API，並在 Visual Studio 2015 中升級資料庫。 我們會使用 [Azure Active Directory 範例](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity)。

若要下載範例專案，請在 Git Bash 中輸入下列命令：

```
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git
```

![顯示如何下載範例專案的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)

若要升級資料庫檔案：

1.  在 Visual Studio 中開啟專案。 在出現的視窗中，會出現一則訊息，說明應用程式需要 SQL Server 2012 Express。 如果您沒有 SQL Server 2012 Express，則需要升級資料庫。  選取 [確定]。

    ![顯示訊息的螢幕擷取畫面，說明應用程式需要 S Q L Server 2012 Express。 否則，您需要升級資料庫。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)

2. 在視窗頂端，選取 [**組建**  >  **組建方案**] 來編譯應用程式。 將會還原所有的 NuGet 套件。

    ![顯示已成功還原 NuGet 套件的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)

3.  在視窗頂端，選取 [ **View**  >  **伺服器總管**]。  在開啟的窗格中，以滑鼠右鍵按一下 [ **資料連線**] 底下的 [ **DefaultConnection** ]，然後選取 [ **修改連接**]。

    ![醒目顯示 [修改連接] 功能表項目的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)

4.  在 [ **修改連接** ] 視窗中，于 [ **資料庫檔案名] (新的或現有的)** 下，選取 **[流覽]**。 輸入 *path\filename.mdf*。 然後，在對話方塊中選取 **[是**]。

    ![顯示用來建立資料庫檔案之對話方塊的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)

5.  在 [ **修改連接** ] 對話方塊中，選取 [ **Advanced**]。

    ![顯示 [Advanced] 按鈕的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)

6.  在 [ **Advanced Properties** ] 對話方塊的 [ **資料來源**] 底下，將 **(LocalDb\v11.0)** 變更為 **(LocalDb) \mssqllocaldb**。

    ![醒目顯示 [資料來源] 欄位的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)

7.  選取 [確定] > [確定]。 然後選取 **[是]** 升級資料庫。

    ![顯示用於升級資料庫之對話方塊的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)

8.  在程式完成之後，請在右側複製 [ **連接字串** ] 欄位中的值。

    ![顯示 [連接字串] 欄位的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)

9.  開啟 *web.config* 檔案，然後將值取代為 `connectionString` 您稍早複製的值。  儲存 *web.config* 檔案。

    > [!NOTE]
    > 上述步驟是必要的，因此您可以取得新的連接字串。 否則，您會在本文稍後執行時收到錯誤 `Update-Database` 。

    ![顯示要在哪裡尋找連接字串值的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)

10. 在 [Visual Studio] 視窗的頂端，選取 [ **View**  >  **Other Windows**  >  **封裝管理員主控台**]。

    ![醒目顯示封裝管理員主控台功能表項目的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)

11. 在 **封裝管理員主控台** ] 窗格中，輸入  `Enable-Migrations` 。

    > [!NOTE]
    > 如果您收到錯誤訊息指出「啟用-無法辨識為 Cmdlet」，請輸入 *安裝套件 EntityFramework* 來更新 entity framework。

    ![顯示要在哪裡進入啟用遷移的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)

12. 輸入 `Add-Migration <AnyNameHere>`。

    ![顯示要在哪裡進入 Add-Migration 測試的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)

13. 輸入 `Update-Database`。

    ![顯示要在哪裡輸入更新資料庫的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)

## <a name="modify-the-web-api"></a>修改 web API 

若要在 Visual Studio 中修改範例 web API：

1.  在 Visual Studio 中，開啟範例。

2.  開啟 *web.config* 檔案。  使用您在 [建立應用程式群組程式](#create-an-application-group) 中所複製的值來修改下列設定：

    -   `ida:ClientId`：輸入用戶端識別碼。

    -   `ida:ClientSecret`：輸入用戶端密碼。

    -   `ida:GraphResourceId`：輸入圖形資源識別碼。

    ![醒目顯示您應該變更之值的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)

3. 在 [ **App_Start**] 下，開啟 *Startup.Auth.cs* 檔案。 進行下列變更：

    -   註解化下列幾行：

        ```
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);
        ```

    -   若要取代移除的行，請新增下列程式程式碼：

        ```
        public static readonly string Authority = "https://<your_fsname>/adfs";
        ```

        在這裡， `<your_fsname>` 將取代為 federation SERVICE URL 的 DNS 部分。 例如，輸入 *adfs.contoso.com*。

        ![顯示啟動點驗證點 C S 檔案中變更的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)

4.  開啟 *UserProfileController.cs* 檔案。 進行下列變更：

    -   批註下列程式程式碼：

        ```
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));
        ```

    -   以下列程式碼取代這兩個專案：

        ```
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));
        ```

        ![顯示使用者設定檔控制器點 C S 檔案變更的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)

    -   批註下列程式程式碼：

        ```
        //authContext = new AuthenticationContext(Startup.Authority);
        ```

    -   以下列程式碼取代這兩個專案：

        ```
        authContext = new AuthenticationContext(Startup.Authority, false);
        ```

        ![醒目顯示對 >aquiretoken 值所做之變更的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)

    -   將下列行的所有實例標記為批註：

        ```
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString() + "/OAuth");
        ```

    -   以下列程式碼取代所有出現專案：

        ```
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString());
        ```

        ![反白顯示 U R I 重新導向 U R I 值的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)

## <a name="test-the-solution"></a>測試解決方案

若要測試機密用戶端解決方案：

1. 在 [Visual Studio] 視窗的頂端，確定已選取 **Internet Explorer** 。 然後選取綠色箭號。

   ![醒目顯示 [Internet Explorer] 按鈕的螢幕擷取畫面。](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)

2. 在開啟的 **ASP.NET** 頁面上： 
    1. 選取右上角的 [ **註冊**]。  
    1. 輸入使用者名稱及密碼。 
    1. 選取 [ **註冊** ]，以在 SQL 資料庫中建立本機帳戶。

   :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG" alt-text="顯示如何在 S Q L 資料庫中建立本機帳戶的螢幕擷取畫面。" lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG":::

3. 請注意 message **Hello abby \@ contoso.com！**。  選取 [ **設定檔**]。

   :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG" alt-text="醒目顯示設定檔的螢幕擷取畫面。" lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG":::

4. 在新的頁面上，您會看到一則訊息，提示您登入。  請 **在這裡** 選取。

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG" alt-text="顯示 [使用者設定檔] 頁面的螢幕擷取畫面。" lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG":::

    系統會提示您登入 AD FS。

   :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG" alt-text="顯示 D F S 登入頁面的螢幕擷取畫面。" lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG":::     
        
## <a name="next-steps"></a>下一步
瞭解 [AD FS 開發](../../ad-fs/AD-FS-Development.md)。
