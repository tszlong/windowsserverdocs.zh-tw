---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: 使用 OAuth 搭配 AD FS 2016 或更新版本，使用 (OBO) 來建立多層式應用程式
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.openlocfilehash: b080f45d36d26ed566eacc5ae1ad84d98dd45d48
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864757"
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016-or-later"></a>使用 OAuth 搭配 AD FS 2016 或更新版本，使用 (OBO) 來建立多層式應用程式

本逐步解說提供的指示可讓您使用 Windows Server 2016 TP5 或更新版本中的 AD FS 來執行 (OBO) 驗證。 若要深入瞭解 OBO authentication，請參閱 [AD FS OpenID Connect/OAuth 流程和應用程式案例](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md)

> [!WARNING]
> 您可以在這裡建立的範例僅供教育之用。 這些指示是為了公開模型的必要元素，最簡單且最基本的實作為可能。 此範例可能不包含錯誤處理和其他相關功能的所有層面，而且只著重在取得成功的 OBO authentication。

## <a name="overview"></a>概觀

在此範例中，我們將建立驗證流程，其中用戶端會存取仲介層 Web 服務，而 Web 服務接著會代表已驗證的用戶端來採取行動，以取得存取權杖。

![代表圖表 AD FS](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.png)

以下是範例將達成的驗證流程
1. 用戶端會向 AD FS 授權端點進行驗證，並要求授權碼
2. 授權端點會將驗證碼傳回到用戶端
3. 用戶端會使用驗證程式代碼，並將其呈現給 AD FS token 端點，以要求仲介層 Web 服務的存取權杖作為 WebAPI
4. AD FS 會將存取權杖傳回至中層 Web 服務。 如需其他功能，中介層服務需要存取後端 WebAPI
5. 用戶端會使用存取權杖來使用中介層服務。
6. 仲介層服務會為 AD FS 權杖端點提供存取權杖，並為已驗證的使用者要求後端 WebAPI 的存取權杖
7. AD FS 會將後端 WebAPI 的存取權杖傳回給仲介層服務 actiing 作為用戶端
8. 仲介層服務使用步驟7中 AD FS 所提供的存取權杖，以用戶端存取後端 WebAPI，並執行必要的功能

## <a name="sample-structure"></a>範例結構

範例將由三個模組組成

模組 | 描述
-------|------------
ToDoClient | 使用者互動的 Native client
ToDoService | 作為後端 WebAPI 用戶端的仲介層 web API
WebAPIOBO | 當使用者新增 ToDoItem 時，ToDoService 用來執行必要作業的後端 web api

## <a name="setting-up-the-development-box"></a>設定開發箱

本逐步解說使用 Visual Studio 2015。 專案大量使用 Active Directory 驗證程式庫 (ADAL) 。 若要瞭解有關 ADAL 的詳細資訊，請參閱 [Active Directory 驗證程式庫 .net](/dotnet/api/microsoft.identitymodel.clients.activedirectory)

此範例也會使用 SQL LocalDB v 11.0。 使用範例之前，請先安裝 SQL LocalDB。

## <a name="setting-up-the-environment"></a>設定環境
我們將使用的基本設定如下：

1. **DC**：將裝載 AD FS 之網域的網域控制站
2. **AD FS 伺服器**：網域的 AD FS 伺服器
3. **開發電腦**：已安裝 Visual Studio 的電腦，將會開發我們的範例

您可以視需要使用兩部電腦。 一個用於 DC/ADFS，另一個用於開發範例。

如何設定網域控制站和 AD FS 已超出本文的範圍。 如需其他部署資訊，請參閱：

- [AD DS 部署](../../ad-ds/deploy/AD-DS-Deployment.md)
- [AD FS 部署](../AD-FS-Deployment.md)

此範例是以針對 Vittorio Bertocci 所建立的 Azure 現有 OBO 範例為基礎，並可在 [這裡](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof)取得。 依照指示，將專案複製到您的開發電腦上，並建立範例的複本以開始使用。

## <a name="clone-or-download-this-repository"></a>複製或下載此存放庫

從殼層或命令列：

```
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git
```

## <a name="modifying-the-sample"></a>修改範例

一旦您開啟 [方案 WebAPI-OnBehalfOf-DotNet]，您會發現方案中有兩個專案。

* **ToDoListClient**：這將作為使用者將與之互動的 OpenID 用戶端
* **ToDoListService**：這會是仲介層 WEB 伺服器應用程式/服務，將與另一個後端互動 WebAPI OBO 已驗證的使用者

如您所見，我們稍後將需要加入另一個專案，作為中介層 ToDoListService 存取的資源。

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>設定用戶端和 Web 服務器應用程式的 AD FS

在目前的範例形式中，驗證是設定為針對 Azure AD 進行。 我們想要變更驗證機制，並將其導向 AD FS 部署在內部部署環境中。 若要這樣做，我們必須設定 AD FS，以辨識範例中的用戶端和 Web 服務器應用程式。

**建立應用程式群組**

開啟 AD FS 管理 MMC，然後新增應用程式群組。 選取原生應用程式-WebAPI 範本。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

按一下 [下一步]，您將會看到可提供用戶端應用程式相關資訊的頁面。 在 AD FS 中，為用戶端應用程式提供適當的名稱。 複製用戶端識別碼，並將它儲存在您稍後可以存取的位置，因為在 visual studio 中的應用程式設定中將需要此識別碼。

>注意：重新導向 URI 可以是任何任意的 URI，因為它在原生用戶端的情況下不會使用

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

按一下 [下一步]，您將會看到可提供 WebAPI 相關資訊的頁面。 為 WebAPI 的 AD FS 專案指定適當的名稱，並輸入重新導向 URI 作為您在 Visual Studio 中看到的 URI ToDoListService

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

按一下 [下一步]，您會看到 [選擇存取控制原則] 頁面。 確定您在 [原則] 區段中看到 [允許每個人]。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

按一下 [下一步]，您會看到 [設定應用程式許可權] 頁面。 在此頁面上，選取預設為 openid (所允許的範圍) 和 user_impersonation。 必須要有範圍 ' user_impersonation '，才能成功地從 AD FS 要求代表存取權杖。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

按 [下一步] 會顯示 [摘要] 頁面。 請流覽嚮導的其餘部分，並完成設定。

為了啟用代理者驗證，我們必須確保 AD FS 會將具有範圍 user_impersonation 的存取權杖傳回給用戶端。 修改 ToDoListServiceWebApi 的宣告發行以包含下列三個自訂規則：

```
@RuleName = "All claims"
c:[]
=> issue(claim = c);

@RuleName = "Issue user_impersonation scope"
=> issue(Type = "http://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");
```

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**將 ToDoListService 新增為應用程式群組中的用戶端**

在這個階段中，我們需要在 AD FS 中建立額外的專案，以供 Web 服務器應用程式作為用戶端，而不是作為資源。 開啟您剛才建立的應用程式群組，然後按一下 [新增應用程式]。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

您將會看到 [新增應用程式至 MySampleGroup] 頁面。 在該頁面上，選取 [伺服器應用程式或網站] 作為獨立應用程式

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

按 [下一步]，您將會看到可提供應用程式詳細資料的頁面。 在 [名稱] 區段中提供適當的設定專案名稱。 請確定用戶端識別碼與 ToDoListServiceWebAPI 的識別碼相同。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

按一下 [下一步]，您將會看到用來設定應用程式認證的頁面。 按一下 [產生共用密碼]。 您將會看到自動產生的秘密。 在某些位置複製秘密，因為我們會在 visual studio 中設定 ToDoListService 時需要此密碼。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

按一下 [下一步] 並完成 wizard。

### <a name="modifying-the-todolistclient-code"></a>修改 ToDoListClient 程式碼

#### <a name="modify-the-application-config"></a>修改應用程式設定

移至您是 WebAPI-OnBehalfOf-DotNet 解決方案中的 ToDoListClient 專案。 開啟 App.config 檔案並進行下列修改

* 批註 ida：租使用者金鑰專案
* 針對 ida： RedirectURI 輸入您在 AD FS 中設定 MySampleGroup_ClientApplication 時所提供的任意 URI。
* 針對 ida： ClientID 索引鍵，請提供在設定 MySampleGroup_ClientApplication 時 AD FS 提供的用戶端識別碼識別碼。
* 針對 ida： ToDoListResourceID 提供您在中設定 ToDoListServiceWebApi 時所提供的資源識別碼 AD FS
* 批註金鑰 ida： AADInstance
* 針對 ida： ToDoListBaseAddress 輸入 ToDoListServiceWebApi 的資源識別碼。 這將會在呼叫 ToDoList WebAPI 時使用。
* 新增金鑰 ida：授權單位，並提供值做為 AD FS 的 URI。

App.Config 中的 **appSettings** 看起來應該像這樣：

```
<appSettings>
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />-->
    <add key="ida:ClientId" value="c7f7b85c-497c-4589-877f-b17a0bd13398" />
    <add key="ida:RedirectUri" value="https://arbitraryuri.com/" />
    <add key="ida:TodoListResourceId" value="https://localhost:44321/" />
    <!--<add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->
    <add key="ida:TodoListBaseAddress" value="https://localhost:44321" />
    <add key="ida:Authority" value="https://fs.anandmsft.com/adfs/"/>
</appSettings>
```

#### <a name="modifying-the-code"></a>修改程式碼

**MainWindow.xaml.cs**

將從應用程式設定讀取租使用者資訊的程式碼批註

```
//private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
//private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
```

將 [字串授權單位] 的值變更為

```
private static string authority = ConfigurationManager.AppSettings["ida:Authority"];
```

變更程式碼以讀取 ToDoListResourceId 和 ToDoListBaseAddress 的正確值

```
private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];
```

在函式 MainWindow ( # A1 變更 >aquiretoken 初始化，如下所示：

```
authContext = new AuthenticationContext(authority, false);
```

### <a name="adding-the-backend-resource"></a>新增後端資源

若要完成代理者流程，您必須建立 ToDoListService 將代表已驗證的使用者存取的後端資源。 後端資源的選擇可能會依需求而有所不同，但基於此範例的目的，您可以建立基本的 WebAPI。

* 以滑鼠右鍵按一下 [方案 WebAPI] 中的方案 ' DotNet '，然後選取 [加入-> 新增專案]
* 選擇 ASP.NET Web 應用程式範本

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* 在下一個提示下，按一下 [變更驗證]
* 選取 [公司和學校帳戶]，然後在右邊的下拉式清單中選取 [內部部署]
* 輸入您 AD FS 部署的 federationmetadata.xml 路徑，並提供應用程式 URI (目前提供任何 URI，您稍後將會變更) 然後按一下 [確定]，將專案新增至方案。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* 以滑鼠右鍵按一下 [方案瀏覽器] 中建立的新專案底下的 [控制器]。 選取 [新增-> 控制器]
* 在範本選取專案中，選取 [Web API 2 控制器-空白]，然後按一下 [確定]。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* 為控制器提供適當的名稱。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* 在控制器中新增下列程式碼：

    ```cs
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Web.Http;
        namespace WebAPIOBO.Controllers
        {
            [Authorize]
            public class WebAPIOBOController : ApiController
            {
                public IHttpActionResult Get()
                {
                    return Ok($"WebAPI via OBO (user: {User.Identity.Name}");
                }
            }
        }
    ```

當有人提出 WebAPI 的 Get 要求時，此程式碼只會傳回字串 WebAPIOBO

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>將新的後端 WebAPI 新增至 AD FS

開啟 MySampleGroup 應用程式群組。 按一下 [新增應用程式] 並選取 [Web API 範本]，然後按一下 [下一步]。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

在 [設定 Web API] 頁面上，提供適當的 WebAPI 專案名稱和識別碼。 識別碼應該是 visual studio 中 WebAPIOBO 專案的 SSL URL 值 (類似于我們針對 BackendWebAPIAdfsAdd) 所做的。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

繼續完成嚮導的其餘部分，就像我們設定 ToDoListService WebAPI 時一樣。 在結束時，您的應用程式群組應該如下所示：

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)

### <a name="modifying-the-todolistservice-code"></a>修改 ToDoListService 程式碼

#### <a name="modifying-the-application-config"></a>修改應用程式設定

* 開啟 Web.config 檔案
* 修改下列金鑰

| Key | 值 |
|:-|:-|
| ida：物件 | 設定 ToDoListService WebAPI 時，提供給 AD FS 的 ToDoListService 識別碼，例如 https://localhost:44321/ |
| ida： ClientID | 設定 ToDoListService WebAPI 時，提供給 AD FS 的 ToDoListService 識別碼，例如 <https://localhost:44321/> </br>**Ida：物件和 ida： ClientID 彼此相符相當重要** |
| ida:ClientSecret | 這是當您在中設定 ToDoListService 用戶端時，AD FS 產生的密碼 AD FS |
| ida： AdfsMetadataEndpoint | 這是您 AD FS 中繼資料的 URL，例如 https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml |
| ida： OBOWebAPIBase | 這是我們將用來呼叫後端 API 的基底位址，例如 https://localhost:44300 |
| ida:Authority | 這是 AD FS 服務的 URL，例如 https://fs.anandmsft.com/adfs/ |

所有其他 ida： **appsettings** 節點中的 >xxxxxxx 索引鍵都可以標記為批註或刪除

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>將驗證從 Azure AD 變更為 AD FS

* 開啟檔案 Startup.Auth.cs
* 移除下列程式碼

    ```
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
    new WindowsAzureActiveDirectoryBearerAuthenticationOptions
    {
        Audience = ConfigurationManager.AppSettings["ida:Audience"],
        Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
        TokenValidationParameters = new TokenValidationParameters{ SaveSigninToken = true }
    });
    ```

    取代為

    ```
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
    ```

#### <a name="modifying-the-todolistcontroller"></a>修改 >controllers\todolistcontroller.cs

新增對副檔名的參考。 取代下列程式碼來修改類別成員

```
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
```

取代為

```
//
// The Client ID is used by the application to uniquely identify itself to Azure AD.
// The client secret is the credentials for the WebServer Client

private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
private static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

// Base address of the WebAPI
private static string OBOWebAPIBase = ConfigurationManager.AppSettings["ida:OBOWebAPIBase"];
```

**修改用於名稱的宣告**

從 AD FS 我們會發行 Nmae 宣告，但我們不會發行 NameIdentifier 宣告。 此範例會在 ToDo 專案中使用 NameIdentifier 來唯一的索引鍵。 為了簡單起見，您可以在程式碼中安全地移除具有 Name 宣告的 NameIdentifier。 尋找並以名稱取代所有出現的 NameIdentifier。

**修改 Post 常式和 CallGraphAPIOnBehalfOfUser ( # B1**

將下列程式碼複製並貼到 ToDoListController.cs 中，並取代 Post 和 CallGraphAPIOnBehalfOfUser 的程式碼

```
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
```

## <a name="running-the-solution"></a>正在執行方案

依預設，visual studio 會設定為在您叫用 debug 時執行一個專案。

* 以滑鼠右鍵按一下方案，然後選取 [屬性]。
* 在 [屬性] 頁面中，選取 [多個啟始專案]，並將這三個專案的動作變更為 [啟動]

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

按 F5 並執行方案

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

按一下 [登入] 按鈕。 系統會提示您使用 AD FS 登入

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

在您登入之後，請在清單中加入待辦事項專案。 在幕後，我們將對 ToDoListService 進行 Post 作業，這會進一步針對 WebAPIOBO web API 進行貼文。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

當作業成功時，您會看到該專案已新增至清單中，並包含來自後端 Web API 的額外訊息，此訊息是使用 OBO auth 流程存取的。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

您也可以在 Fiddler 上查看詳細的追蹤。 啟動 Fiddler 並啟用 HTTPS 解密。 您可以看到我們對/adfs/oautincludes 端點提出兩個要求。
在第一次互動中，我們會向權杖端點出示存取權杖，並取得 AD FS 的存取權杖 https://localhost:44321/ ![ OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

在第二次與權杖端點互動的情況下，您可以看到我們 **requested_token_use** 設定為 **on_behalf_of** ，而我們使用為仲介層 web 服務取得的存取權杖，也就是用 https://localhost:44321/ 來取得代表權杖的判斷提示。
![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)
