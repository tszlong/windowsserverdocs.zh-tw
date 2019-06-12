---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: 建置使用代理者 (OBO) 使用 OAuth 與 AD FS 2016 或更新版本的多層式應用程式
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 047f297cfaabff3cbbd45057a4198e2fd2e747de
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445457"
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016-or-later"></a>建置使用代理者 (OBO) 使用 OAuth 與 AD FS 2016 或更新版本的多層式應用程式


本逐步解說提供實作上-代表 (OBO) 驗證使用 AD FS，在 Windows Server 2016 TP5 或更新版本的指示。 若要深入了解 OBO 驗證，請閱讀[適用於開發人員的 AD FS 案例](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)

>警告：您可以在這裡建置的範例是僅供教育。 這些指示是最簡單、 最小實作能夠公開 （expose） 模型的必要項目。 此範例可能不會包含錯誤處理的所有層面，以及其他與關聯的功能並將焦點放僅在取得 OBO 驗證成功。

## <a name="overview"></a>總覽

在此範例中，我們將建立的驗證流程，其中用戶端會存取中介層 Web 服務和 web 服務接著會代表已驗證的用戶端取得存取權杖。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.png)

以下是範例，以達到的驗證流程
1. 用戶端會向 AD FS 授權端點，並要求授權碼
2. 授權端點傳回至用戶端的驗證碼
3. 用戶端使用驗證碼，與對 AD FS 權杖端點要求存取權杖中呈現為 WebAPI 的中間層 Web 服務
4. AD FS 會傳回存取權杖，中間層 Web 服務。 如需其他功能中, 介層服務需要存取後端 WebAPI
5. 用戶端會使用存取權杖，使用中介層服務。
6. 中介層服務提供 AD FS 權杖的結束點的存取權杖，並要求存取權杖的後端 WebAPI 代表的已驗證的使用者
7. AD FS 作為用戶端將後端 WebAPI 的存取權杖傳回給中介層服務 actiing
8. 中介層服務會使用在步驟 7 中的 AD FS 所提供的存取權杖存取後端為用戶端的 WebAPI 並執行必要的功能

## <a name="sample-structure"></a>範例結構

範例會包含三個模組


模組 | 描述
-------|------------
ToDoClient | 與使用者互動的原生用戶端
ToDoService | 中間層 web API 做為後端 WebAPI 的用戶端
WebAPIOBO | 後端 web api，供 ToDoService 用來執行必要的作業，當使用者新增的 ToDoItem 時




## <a name="setting-up-the-development-box"></a>設定開發電腦

此逐步解說會使用 Visual Studio 2015。 專案會大量使用 Active Directory Authentication Library (ADAL)。 若要了解 ADAL，請閱讀[Active Directory Authentication Library.NET](https://msdn.microsoft.com/library/azure/mt417579.aspx)

此範例也會使用 SQL LocalDB v11.0。 安裝 SQL LocalDB 之前的範例。

## <a name="setting-up-the-environment"></a>設定環境
我們將使用的基本安裝：

1. **DC**:將在其中裝載 AD FS 的網域的網域控制站
2. **AD FS 伺服器**:網域的 AD FS 伺服器
3. **開發電腦**:我們已安裝 Visual Studio 和開發我們的範例中的機器

您可以如果您想，使用只有兩部機器。 一個用於 DC/ADFS，另一個則用於開發的範例。

如何設定網域控制站和 AD FS 已超出本文的範圍。 其他部署資訊，請參閱：

- [AD DS 部署](../../ad-ds/deploy/AD-DS-Deployment.md)
- [AD FS 部署](../AD-FS-Deployment.md)

此範例是根據現有 OBO 範例針對 Azure 建立 Vittorio bertocci 且可用[此處](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof)。 請依照下列指示來複製您的開發電腦上的專案，並建立一份以開始使用的範例。

## <a name="clone-or-download-this-repository"></a>複製或下載此存放庫

從您的殼層或命令列：

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>修改範例

當您開啟方案 WebAPI-OnBehalfOf-DotNet.sln 時，您會注意到您的方案中有兩個專案

* **ToDoListClient**:這將做為使用者進行互動的 OpenID 用戶端
* **ToDoListService**:這會是中介層 web 伺服器應用程式 / 服務，將會與另一個後端 WebAPI OBO 互動已驗證的使用者

如您所見，我們必須新增另一個專案稍後會做為將中介層 ToDoListService 所存取的資源。

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>在用戶端和 web 伺服器應用程式中設定 AD FS

此範例的最新的格式，必須針對 Azure AD 來設定驗證。 我們想要變更的驗證機制，並直接它針對 AD FS 在內部部署。 若要這樣做，我們要設定來辨識用戶端的 AD FS 和 web 伺服器應用程式，我們在此範例中。

**建立應用程式群組**

開啟 AD FS 管理 MMC 並加入新的應用程式群組。 選取原生-應用程式-WebAPI 範本。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

按一下 [下一步]，您會看到的頁面提供用戶端應用程式的相關資訊。 用戶端應用程式在 AD FS 中提供適當的名稱。 複製用戶端識別碼，並將它儲存在稍後這會需要在 visual studio 中的應用程式設定中，您可以存取的地方。

>注意:重新導向 URI 可以是任何任意的 URI，因為它真的不會發生原生用戶端

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

按一下 [下一步]，您會看到的頁面提供 WebAPI 的相關資訊。 針對 WebAPI 提供適合的名稱，AD FS 項目和輸入的重新導向 URI 的 URI，您會看到 Visual Studio 中針對為 ToDoListService

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

按一下 [下一步]，您會看到 [選擇存取控制原則] 頁面。 請確定您會看到 「 允許每個人 」 的原則區段中。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

按一下 [下一步]，您會看到 [設定應用程式權限] 頁面。 這個頁面上，選取 允許的範圍 （預設選項） 的 openid 以及 user_impersonation。 範圍 'user_impersonation' 是為了能夠成功地從 AD FS 要求代表的存取權杖。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

按一下 下一步將會顯示 摘要 頁面。 瀏覽精靈的其餘部分並完成相關設定。

若要讓代表的驗證，我們需要確保 AD FS 會傳回給用戶端 user_impersonation 範圍的存取權杖。 修改針對包含下列三個自訂規則的 ToDoListServiceWebApi 宣告發行：

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**做為用戶端應用程式群組中新增 ToDoListService**

在這個階段中，我們需要建立做為用戶端並不只是做為資源的 web 伺服器應用程式的 AD FS 中的其他項目。 開啟您剛建立的應用程式群組，然後按一下 新增應用程式。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

您會看到 [新增 MySampleGroup 新的應用程式] 頁面。 在該頁面上，選取 「 伺服器應用程式或網站 」 當做獨立應用程式

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

按一下 下一步，您會看到頁面，即可提供應用程式詳細資料。 提供組態項目，在 [名稱] 區段中的適當名稱。 請確定用戶端識別碼是與 ToDoListServiceWebAPI 的識別項相同

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

按一下 [下一步]，您會看到 [設定應用程式的認證] 頁面。 按一下 [產生共用的密碼]。 您會看到使用自動產生的密碼。 因為這是需要我們在 visual studio 中設定 ToDoListService 時，請將複製的某處的祕密。


![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

按一下 [下一步]，並完成精靈。

### <a name="modifying-the-todolistclient-code"></a>修改 ToDoListClient 程式碼

#### <a name="modify-the-application-config"></a>修改應用程式組態

移至您在 ToDoListClient 專案 WebAPI OnBehalfOf DotNet 方案中。 開啟 App.config 檔案，並進行下列修改

* 註解的 ida： 租用戶索引鍵的項目
* Ida: RedirectURI 輸入您在 AD FS 中設定 MySampleGroup_ClientApplication 時所提供的任意 URI。
* 對於 ida: ClientID 金鑰，提供用戶端設定 MySampleGroup_ClientApplication 時提供的 AD FS 的識別碼辨識符號。
* 您為 AD FS 中設定 ToDoListServiceWebApi 時 ida: ToDoListResourceID 提供的資源識別碼
* 註解主要的 ida: AADInstance
* Ida: ToDoListBaseAddress 輸入 ToDoListServiceWebApi 的資源識別碼。 這會呼叫 ToDoList WebAPI 時使用。
* 新增金鑰的 ida： 授權單位，並提供作為 URI 的值，適用於 AD FS。

您**appSettings** App.Config 中應該看起來像這樣：

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

註解讀取應用程式組態中的租用戶的資訊列

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

字串權限的值變更

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

程式碼，以讀取 ToDoListResourceId 和 ToDoListBaseAddress 的正確值

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

函式中 mainwindow （） 會變更為 aquiretoken 初始化：

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>新增後端資源

若要完成代理者的流程，您必須建立 ToDoListService 將用來存取後端資源代表的已驗證的使用者。 根據需求，而異的後端資源的選擇，但在此範例中，您可以建立基本的 WebAPI。

* 解決方案 'OnBehalfOf-Webapi-dotnet' 方案總管] 中以滑鼠右鍵按一下並選取 [新增]-> [新增專案
* 選擇 ASP.NET Web 應用程式範本

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* 在下一個提示字元上按一下 [變更驗證]
* 選取 工作和學校帳戶，然後在右側下拉式清單中，選取 ' 內部 '
* 輸入您的 AD FS 部署 federationmetadata.xml 路徑，並提供應用程式 URI （到目前為止，提供任何 URI 與您將在稍後變更），按一下 [確定] 以將專案加入方案。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* 以滑鼠右鍵按一下控制站上建立新專案下的 [方案總管] 中。 選取 新增-> 控制器
* 在 範本選取範圍中，選取 'Web API 2 控制器-空白' 並按一下 確定。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* 提供適當的控制器名稱

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* 在控制器中加入下列程式碼


~~~
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
~~~

此程式碼只會傳回字串，當任何人都針對 WebAPI WebAPIOBO 將 Get 要求

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>將新的 WebAPI 後端新增至 AD FS

開啟 MySampleGroup 應用程式群組。 按一下 新增應用程式並選取 Web API 範本，並按一下 下一步。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

在 [設定 Web API] 頁面上提供適當名稱的 WebAPI 項目和識別項。 識別碼應該是從 WebAPIOBO 專案 SSL URL 值，在 visual studio 中 （類似於我們的 BackendWebAPIAdfsAdd）。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

繼續執行精靈的其餘部分相同為我們設定 ToDoListService WebAPI 的時。 結束您的應用程式群組看起來應該像下面：

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>修改 ToDoListService 程式碼

#### <a name="modifying-the-application-config"></a>修改應用程式組態

* 開啟 Web.config 檔案
* 修改下列索引鍵

| Key                      | 值                                                                                                                                                                                                                   |
|:-------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ida:Audience             | 指定在設定 ToDoListService WebAPI，比方說，AD fs ToDoListService 的識別碼 https://localhost:44321/                                                                                         |
| ida:ClientID             | 指定在設定 ToDoListService WebAPI，比方說，AD fs ToDoListService 的識別碼 <https://localhost:44321/> </br>**它是非常重要的 ida： 對象和 ida: ClientID 符合每個其他** |
| ida:ClientSecret         | 這是當您在 AD FS 中設定 ToDoListService 用戶端時，AD FS 將會產生的密碼                                                                                                                   |
| ida:AdfsMetadataEndpoint | 這是您的 AD FS 中繼資料 URL 如例如 https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml                                                                                             |
| ida:OBOWebAPIBase        | 這是我們將使用後端 API，例如呼叫的基底地址 https://localhost:44300                                                                                                                     |
| ida:Authority            | 這是您的 AD FS 服務的 URL 範例 https://fs.anandmsft.com/adfs/                                                                                                                                          |

中索引鍵的所有其他 ida: XXXXXXX **appsettings**可以標記為註解或刪除節點

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>從 Azure AD 到 AD FS 的變更驗證

* 開啟檔案 Startup.Auth.cs
* 移除下列程式碼

        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
                TokenValidationParameters = new TokenValidationParameters{ SaveSigninToken = true }
            });

取代為

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

新增 System.Web.Extensions 的參考。 取代下列程式碼來修改類別成員

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

取代為

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The client secret is the credentials for the WebServer Client

    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

    // Base address of the WebAPI
    private static string OBOWebAPIBase = ConfigurationManager.AppSettings["ida:OBOWebAPIBase"];

**修改用於名稱宣告**

從 AD FS 中，我們要發出 Nmae 宣告，但我們不會發出 NameIdentifier 宣告。 此範例會使用 NameIdentifier 唯一 ToDo 項目中的索引鍵。 為了簡單起見，您可以安全地在程式碼中移除具有名稱宣告 NameIdentifier。 尋找和取代所有出現的 NameIdentifier 具有名稱。

**修改後的常式和 CallGraphAPIOnBehalfOfUser()**

複製並貼上下列程式碼 ToDoListController.cs，然後將 Post 和 CallGraphAPIOnBehalfOfUser 取代程式碼

    // POST api/todolist
    public async Task Post(TodoItem todo)
    {
      if (!ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value.Contains("user_impersonation"))
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
        // The Resource ID of the service we want to call.
        // The current user's access token, from the current request's authorization header.
        // The credentials of this application.
        // The username (UPN or email) of the user calling the API
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
                    result = await authContext.AcquireTokenAsync(OBOWebAPIBase, clientCred, userAssertion);
                    //result = await authContext.AcquireTokenAsync(...);
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

## <a name="running-the-solution"></a>執行解決方案


根據預設 visual studio 會執行一個專案，當您叫用來執行的偵錯設定。

* 以滑鼠右鍵按一下方案，然後選取 [屬性]。
* 在 [屬性] 頁面中選取多個啟始專案，並變更啟動所有的三個項目的動作。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

按 f5 鍵，並執行解決方案

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

按一下 [登入] 按鈕。 系統會提示您使用 AD FS 登入

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

之後您登入、 在清單中新增 ToDo 項目。 在幕後我們要來進一步執行 WebAPIOBO web API 的 Post ToDoListService 執行 Post 作業。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

在成功的作業中，您會看到的項目已從後端 Web API 已存取使用 OBO 驗證流程新增至具有額外的訊息的清單。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

您也可以在 Fiddler 中看到詳細的追蹤。 啟動 Fiddler，並啟用 HTTPS 解密。 您可以看到我們提出 /adfs/oautincludes 端點的兩個要求。
在第一次的互動，方法，我們可以呈現存取程式碼，對權杖端點，並取得存取權杖以供 https://localhost:44321/ ![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

在第二個語彙基元端點互動，您可以看到我們有**requested_token_use**設定為**on_behalf_of**而且我們會使用為中介層 web 服務，也就是取得存取權杖 https://localhost:44321/以取得代表的權杖判斷提示。
![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
