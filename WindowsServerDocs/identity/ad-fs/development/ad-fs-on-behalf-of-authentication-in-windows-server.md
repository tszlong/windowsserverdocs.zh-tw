---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: "組建多層應用程式使用 On-Behalf-Of (OBO) 使用 AD FS 2016 中的 OAuth"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8940cde2b78ce3ead499263e6fba0fbe28aae695
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016"></a>組建多層應用程式使用 On-Behalf-Of (OBO) 使用 AD FS 2016 中的 OAuth

>適用於：Windows Server 2016

本節提供實作開展工作的 (OBO) 驗證的 Windows Server 2016 TP5 AD FS 使用的指示。  O 深入了解 OBO 驗證請朗讀[適用於開發人員 AD FS 案例](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)

>警告：僅供教育是您可以在此組建的範例。 這些指示僅適用於可能公開所需項目模式的最簡單、最小實作。 範例可能不會包含錯誤處理的一切和其他相關的功能和聚焦只有在收到成功 OBO 驗證。

## <a name="overview"></a>概觀

在此範例中，我們將建立驗證流程位置 client 存取中間層 Web 服務和 web 服務則將會執行代表證取得存取預付碼。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.PNG)

以下是範例達成驗證流程
1. Client 驗證，AD FS 授權結束點，並要求驗證碼
2. 授權端點回到 client 的驗證碼
3. Client 使用驗證碼，並將其給 AD FS 權杖端點要求存取權杖展示 WebAPI 為中間層 Web 服務
4. AD FS 回到中型層 Web 服務的存取預付碼。 需要存取端 WebAPI 中央層服務的額外功能，
5. Client 使用中央層服務使用存取預付碼。
6. 中央層服務提供 AD FS 權杖結束點存取預付碼和要求存取預付碼的端 WebAPI 開展工作的已驗證的使用者
7. AD FS 回到端 WebAPI 存取權杖中央層服務 actiing client 為
8. 中央層服務使用 AD FS 執行「步驟 7」中所提供的存取憑證存取端為 client WebAPI 並執行所需的功能

## <a name="sample-structure"></a>範例結構

範例會所組成的三個模組


模組 | 描述
-------|------------
ToDoClient | 原生 client 與使用者互動
ToDoService | 中間層 web 做為 client WebAPI 端的 API
WebAPIOBO | 端網頁，用來執行必要的作業時的使用者新增 ToDoItem ToDoService api




## <a name="setting-up-the-development-box"></a>開發方塊設定

這個解說使用 Visual Studio 2015。 專案經驗會使用 Active Directory 驗證媒體櫃 (ADAL)。 若要深入了解 ADAL 請朗讀[Active Directory 驗證庫.NET](https://msdn.microsoft.com/library/azure/mt417579.aspx)

也會使用 SQL LocalDB v11.0。 安裝之前處理範例 SQL LocalDB。

## <a name="setting-up-the-environment"></a>設定環境
我們將會使用的基本安裝：

1. **DC**：網域控制站 AD FS 可裝載的網域
2. **AD FS 伺服器**: AD FS 伺服器的網域
3. **開發電腦**：電腦我們已安裝 Visual Studio，將會開發我們範例

您可以如果您想要使用只兩部電腦。 另一個用於俠日 ADFS，另一個用於範例開發無障礙。

如何設定的網域控制站和 AD FS 超出範圍的這篇文章。 適用於其他部署的資訊查看：

- [AD DS 部署](../../ad-ds/deploy/AD-DS-Deployment.md)
- [AD FS 部署](../AD-FS-Deployment.md)

範例已依據針對 Azure Vittorio Bertocci 所建立的現有 OBO 範例也可以使用[在此](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof)。 請依照下列指示您開發電腦上的專案的複製和建立的範例，若要開始使用的複本。

## <a name="clone-or-download-this-repository"></a>複製或下載這個存放庫

從命令列或殼層：

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>修改範例

當您開放 WebAPI-OnBehalfOf-DotNet.sln 方案，您會注意到您有兩個專案中的方案

* **ToDoListClient**：這樣做為 OpenID client 的使用者會互動
* **ToDoListService**：這會將中間層 web 伺服器上的應用程式] / [服務，將會與其他端 WebAPI OBO 互動驗證的使用者

您可以看到時，我們需要新增另一個專案稍後將會做為中間層 ToDoListService 會存取資源。

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>AD FS 設定 Client 與 web 伺服器上的應用程式

在目前的形式的範例，是針對 Azure AD 設定驗證。 我們想要變更的驗證機制，直接向 AD FS 它部署場所上。 若要這樣做，我們需要設定辨識 client AD FS 與 web 伺服器上的應用程式我們在此範例中。

**建立應用程式群組**

打開 AD FS 管理 MMC，並加入新的應用程式群組。 選取 WebAPI-應用程式原生-範本。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

按一下 [下一步，您將會看到提供 Client 的應用程式的相關資訊的頁面。 給 AD FS 應用程式 client 適當的名稱。 複製 client 識別碼，並儲存這將會需要 visual studio 中的應用程式設定中之後，您可以存取位置。

>注意：在該真的不是在原生戶端重新導向 URI 可以是任何任意 URI

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

按 [下一步，您將會看到提供 WebAPI 的相關資訊的頁面。 命名適合 AD FS 項目的的 WebAPI 並輸入您看到 Visual Studio 中 ToDoListService URI 重新導向 URI

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

按一下 [下一步，您將會看到 [選擇存取控制] 原則頁面。 請確定您看到 [允許所有人] 原則一節中。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

按 [下一步，您將會看到的 [設定] 應用程式權限頁面。 在這個頁面上，選取 [允許的範圍 openid（預設值，選取）和 user_impersonation。 範圍 'user_impersonation' 是必要無法順利向 AD FS 要求開展工作的存取權的憑證。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

旁邊就會顯示 [摘要] 頁面。 瀏覽精靈中的其餘部分，並完成設定。

為了讓開展工作的驗證，我們需要確定 AD FS 傳回至 client 的範圍 user_impersonation 存取預付碼。 修改為包含下列三種自訂規則 ToDoListServiceWebApi 宣告發行：

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue open id scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "openid");

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**新增 ToDoListService 為 client 應用程式群組中**

在此階段我們需要中為 web 伺服器上的應用程式做為 client，而不只是資源 AD FS 進行其他項目。 打開應用程式群組您剛建立並按一下 [新增應用程式。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

您會看到 [新增新的應用程式來 MySampleGroup] 頁面。 該頁面上，選取 [伺服器應用程式或網站」的獨立應用程式

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

按 [下一步，您將會看到提供應用程式的詳細資料頁面。 提供設定中的項目名稱] 區段中適用的名稱。 確保 Client 識別碼相同的 ToDoListServiceWebAPI id

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

按 [下一步，您將會看到與設定的應用程式認證頁面。 按一下 [產生共用的密碼]。 您會看到所自動密碼。 這是必要時我們在 visual studio 設定 ToDoListService 複製某些位置的密碼。


![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

按 [下一步，完成精靈。

### <a name="modifying-the-todolistclient-code"></a>修改 ToDoListClient 的程式碼

#### <a name="modify-the-application-config"></a>修改應用程式設定

移至您在 ToDoListClient 專案中 WebAPI-OnBehalfOf-DotNet 方案。 打開 App.config 檔案，並進行下列修改

* 加 ida：承租人金鑰的項目
* 針對 ida: RedirectURI 輸入您在 AD FS 設定 MySampleGroup_ClientApplication 時所提供的任意 URI。
* 對於 ida: ClientID 金鑰，提供 client ID 識別碼設定 MySampleGroup_ClientApplication 時，給 AD FS。
* 您的 ida: ToDoListResourceID 提供的資源 ID 提供 AD FS 中設定 ToDoListServiceWebApi 時
* 主要 ida: AADInstance 意見
* 輸入 ida: ToDoListBaseAddress ToDoListServiceWebApi ID 資源。 這將會通話 ToDoList WebAPI 時使用。
* 新增 ida：授權金鑰，並提供 AD FS URI 值。

您**和 appSettings**中 App.Config 應該看起來像這樣：

    <appSettings>
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />-->
    <add key="ida:ClientId" value="c7f7b85c-497c-4589-877f-b17a0bd13398" />
    <add key="ida:RedirectUri" value="https://arbitraryuri.com/" />
    <add key="ida:TodoListResourceId" value="https://localhost:44321/" />
    <!--<add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->
    <add key="ida:TodoListBaseAddress" value="https://localhost:44321" />
    <add key="ida:Authority" value="https://fs.anandmsft.com/adfs/"/>
    </appSettings>

#### <a name="modifying-the-code"></a>修改程式碼

**MainWindow.xaml.cs**

加行朗讀承租人資訊的應用程式設定

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

將值字串的權限的變更

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

若要朗讀 ToDoListResourceId 和 ToDoListBaseAddress 正確的值的程式碼變更

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

在 [函式 MainWindow() 變更 authcontext 初始化為：

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>新增端資源

您必須完成開展工作的流程，才能建立端資源將會存取 ToDoListService 開展工作的已驗證的使用者。 選擇端資源會依需求，但針對此範例中，您可以建立基本的 WebAPI。

* 方案 ' WebAPI-OnBehalfOf-DotNet' 方案檔案總管中按一下滑鼠右鍵，選取 [新增]-> [新增專案
* 選擇 ASP.NET Web 應用程式範本

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* 在下一步提示，按一下 [' 變更驗證]
* 選取 [工作和學校帳號]，然後選取 [上正確下拉式清單的 [' 先 '
* 輸入您的部署 AD FS federationmetadata.xml 路徑，並提供的應用程式 URI（提供任何 URI 現在，以及您將會在稍後變更），按一下 [新增專案方案確定。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* 按一下滑鼠右鍵在控制器上建立的新專案在總管] 中。 選取 [新增]-> [控制器
* 選取範本，選取 ['Web API 2 控制器-空白'，按一下 [確定]。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* 命名適當的控制器

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* 在控制器中新增下列程式碼


        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Web.Http;
        namespace WebAPIOBO.Controllers
        {
            public class WebAPIOBOController : ApiController
            {
                public IHttpActionResult Get()
                {
                    return Ok("WebAPI via OBO");
                }
            }
        }

任何人的 WebAPI WebAPIOBO 放取得要求時，此驗證碼只會傳回字串

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>AD FS 以新增新的端 WebAPI

打開 MySampleGroup 應用程式群組。 按一下 [新增應用程式，選取 Web API 範本，按一下 [下一步]

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

在 [設定網路 API 頁面上提供 WebAPI 項目以及適當的名稱。 在 visual studio（類似於我們針對 BackendWebAPIAdfsAdd）識別碼應該 WebAPIOBO 專案的值 SSL URL。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

請繼續透過精靈中的其餘部分相同為我們設定 ToDoListService WebAPI 時。 結尾您的應用程式群組應該的外觀如下：

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>修改 ToDoListService 的程式碼

#### <a name="modifying-the-application-config"></a>修改應用程式設定

* 打開 web.config
* 修改下鍵

| 鍵 | 值。 |
|:-----|:-------|
|對象 ida:| 為您提供給 AD FS 設定 ToDoListService WebAPI，例如時 https://localhost:44321/ ToDoListService 的來電顯示|
|ida: ClientID| 為您提供給 AD FS 設定 ToDoListService WebAPI，例如時 https://localhost:44321/ ToDoListService 的來電顯示 </br>**請務必 ida: ClientID 與 ida：對象符合彼此**|
|ida: ClientSecret| 這是 AD FS 產生當您已設定 ToDoListService client AD FS 中的密碼|
|ida: ADFSMetadata| 這是您 AD FS 的中繼資料，例如 https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml URL|
|ida: OBOWebAPIBase| 這是我們用來呼叫端的 API，例如 https://localhost:44300 的基底地址|
|授權單位 ida:| 這是您 AD FS 服務，https://fs.anandmsft.com/adfs/ 的範例 URL|


所有其他 ida: XXXXXXX 鍵在 [**和 appsettings**節點可以略過或刪除

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>Azure AD 來 AD FS 從變更驗證

* 打開 Startup.Auth.cs 檔案
* 移除下列程式碼

        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
                TokenValidationParameters = new TokenValidationParameters{ SaveSigninToken = true }
            });

使用

        app.UseActiveDirectoryFederationServicesBearerAuthentication(
            new ActiveDirectoryFederationServicesBearerAuthenticationOptions
            {
                MetadataEndpoint = ConfigurationManager.AppSettings["ida:AdfsMetadataEndpoint"],
                TokenValidationParameters = new TokenValidationParameters()
                {
                    SaveSigninToken = true,
                    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"]
                }
            });

#### <a name="modifying-the-todolistcontroller"></a>修改 ToDoListController

加入 System.Web 參考。擴充功能。 更換下列程式碼來修改課程成員

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The App Key is a credential used by the application to authenticate to Azure AD.
    // The Tenant is the name of the Azure AD tenant in which this application is registered.
    // The AAD Instance is the instance of Azure, for example public Azure or Azure China.
    // The Authority is the sign-in URL of the tenant.
    //
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string appKey = ConfigurationManager.AppSettings["ida:AppKey"];

    //
    // To authenticate to the Graph API, the app needs to know the Grah API's App ID URI.
    // To contact the Me endpoint on the Graph API we need the URL as well.
    //
    private static string graphResourceId = ConfigurationManager.AppSettings["ida:GraphResourceId"];
    private static string graphUserUrl = ConfigurationManager.AppSettings["ida:GraphUserUrl"];
    private const string TenantIdClaimType = "https://schemas.microsoft.com/identity/claims/tenantid";

使用

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The client secret is the credentials for the WebServer Client

    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

    // Base address of the WebAPI
    private static string OBOWebAPIBase = ConfigurationManager.AppSettings["ida:OBOWebAPIBase"];

**修改用於名稱宣告**

我們從 AD FS 發行 Nmae 理賠要求，但是我們不發行 NameIdentifier 理賠要求。 範例使用 NameIdentifier 唯一 ToDo 項目] 中的金鑰。 為了簡化，您可以放心密碼移除 NameIdentifier 與名稱宣告。 尋找和所有場次 NameIdentifier 都取代名稱。

**修改 CallGraphAPIOnBehalfOfUser() 文章程序**

複製和貼上 ToDoListController.cs 下列程式碼的文章與 CallGraphAPIOnBehalfOfUser 取代的程式碼

    // POST api/todolist
    public async Task Post(TodoItem todo)
    {
        if (!ClaimsPrincipal.Current.FindFirst("https://schemas.microsoft.com/identity/claims/scope").Value.Contains("user_impersonation"))
        {
            throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found" });
        }

        //
        // Call the WebAPIOBO On Behalf Of the user who called the To Do list web API.
        //
        string augmentedTitle = null;
        string custommessage = await CallGraphAPIOnBehalfOfUser();
        if (custommessage != null)
        {
            augmentedTitle = String.Format("{0}, Message: {1}", todo.Title, custommessage);
        }
        else
        {
            augmentedTitle = todo.Title;
        }

        if (null != todo && !string.IsNullOrWhiteSpace(todo.Title))
        {
            db.TodoItems.Add(new TodoItem { Title = augmentedTitle, Owner = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value });
            db.SaveChanges();
        }
    }

    public static async Task<string> CallGraphAPIOnBehalfOfUser()
    {
        string accessToken = null;
        AuthenticationResult result = null;
        AuthenticationContext authContext = null;
        HttpClient httpClient = new HttpClient();
        string custommessage = "";

        //
        // Use ADAL to get a token On Behalf Of the current user.  To do this we will need:
        //      The Resource ID of the service we want to call.
        //      The current user's access token, from the current request's authorization header.
        //      The credentials of this application.
        //      The username (UPN or email) of the user calling the API
        //
        ClientCredential clientCred = new ClientCredential(clientId, clientSecret);
        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        string userName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn) != null ? ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value : ClaimsPrincipal.Current.FindFirst(ClaimTypes.Email).Value;
        string userAccessToken = bootstrapContext.Token;
        UserAssertion userAssertion = new UserAssertion(bootstrapContext.Token, "urn:ietf:params:oauth:grant-type:jwt-bearer", userName);

        string userId = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
        authContext = new AuthenticationContext(authority, false);

        // In the case of a transient error, retry once after 1 second, then abandon.
        // Retrying is optional.  It may be better, for your application, to return an error immediately to the user and have the user initiate the retry.
        bool retry = false;
        int retryCount = 0;

        do
        {
            retry = false;
            try
            {
                result = authContext.AcquireToken(OBOWebAPIBase, clientCred, userAssertion);
                accessToken = result.AccessToken;
            }
            catch (AdalException ex)
            {
                if (ex.ErrorCode == "temporarily_unavailable")
                {
                    // Transient error, OK to retry.
                    retry = true;
                    retryCount++;
                    Thread.Sleep(1000);
                }
            }
        } while ((retry == true) && (retryCount < 1));

        if (accessToken == null)
        {
            // An unexpected error occurred.
            return (null);
        }

        // Once the token has been returned by ADAL, add it to the http authorization header, before making the call to access the To Do list service.
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

        // Call the WebAPIOBO.
        HttpResponseMessage response = await httpClient.GetAsync(OBOWebAPIBase + "/api/WebAPIOBO");


        if (response.IsSuccessStatusCode)
        {
            // Read the response and databind to the GridView to display To Do items.
            string s = await response.Content.ReadAsStringAsync();
            JavaScriptSerializer serializer = new JavaScriptSerializer();
            custommessage = serializer.Deserialize<string>(s);
            return custommessage;
        }
        else
        {
            custommessage = "Unsuccessful OBO operation : " + response.ReasonPhrase;
        }
        // An unexpected error occurred calling the Graph API.  Return a null profile.
        return (null);
    }

## <a name="running-the-solution"></a>執行方案


Visual studio 預設設定來執行一個專案，當您遇到執行偵錯。

* 以滑鼠右鍵按一下方案，然後選取 [屬性。
* 屬性網頁中選取多個開機專案及變更動作到開始畫面的所有的三個項目。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

F5 並執行方案

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

按一下 [登入] 按鈕。 將會提示您登入使用 AD FS

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

在您登入，新增 ToDo 項目清單中。 我們會想要的進一步即可張貼到 WebAPIOBO web API ToDoListService 文章作業幕後的細節。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

在順利運作，您會看到的項目已新增至其他訊息清單後端使用 OBO 驗證流程已存取網路 API。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

您也可以查看詳細的追蹤 Fiddler 上。 上市 Fiddler 以及 HTTPS 解密。 您可以看到我們對 /adfs/oautincludes 端點的兩個要求。
中的第一個互動，我們提供權杖端點存取程式碼，並取得 https://localhost:44321/ 權杖存取![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

在第二個權杖端點互動，您可以看到我們已經**requested_token_use**設定為**on_behalf_of**，並使用存取權杖取得中間層 web 服務，亦即 https://localhost:44321/ 為宣告，來取得開展工作的預付碼。
![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
