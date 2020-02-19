---
title: 使用 OAuth 和 ADAL 建立單一頁面 web 應用程式。JS 與 AD FS 2016 或更新版本
description: 此逐步解說提供的指示，說明如何使用適用于 JavaScript 的 ADAL 保護 AngularJS 的單一頁面應用程式，以進行驗證 AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/13/2018
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.openlocfilehash: f4973da0d9e0c347cff8fc910f96277055b66dec
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465542"
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016-or-later"></a>使用 OAuth 和 ADAL 建立單一頁面 web 應用程式。JS 與 AD FS 2016 或更新版本

本逐步解說提供的指示，說明如何使用適用于 JavaScript 的 ADAL 來驗證 AD FS，以保護 AngularJS 為基礎的單一頁面應用程式，並以 ASP.NET Web API 後端執行。

在此案例中，當使用者登入時，JavaScript 前端會使用[javascript 的 Active Directory 驗證程式庫（ADAL）。JS）](https://github.com/AzureAD/azure-activedirectory-library-for-js)和隱含授權授與，以從 Azure AD 取得識別碼權杖（id_token）。 系統會快取權杖，而且用戶端會在呼叫其 Web API 後端（使用 OWIN 中介軟體來保護）時，將其附加至要求做為持有人權杖。

>[!IMPORTANT]
>您可以在這裡建立的範例僅供教育目的之用。 這些指示適用于公開模型所需元素的最簡單且最基本的執行方式。 此範例可能不包含錯誤處理和其他相關功能的所有層面。

>[!NOTE]
>本逐步解說**僅**適用于 AD FS Server 2016 和更新版本 

## <a name="overview"></a>概觀
在此範例中，我們將建立驗證流程，其中單一頁面應用程式用戶端會對 AD FS 進行驗證，以安全存取後端上的 WebAPI 資源。 以下是整體驗證流程


![AD FS 授權](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

使用單一頁面應用程式時，使用者會流覽至起始位置，其中會載入起始頁和 JavaScript 檔案集合和 HTML 視圖。 您需要設定 Active Directory 驗證程式庫（ADAL）來知道應用程式的重要資訊，例如，AD FS 實例的用戶端識別碼，以便將驗證導向您的 AD FS。

如果 ADAL 看到驗證的觸發程式，它會使用應用程式所提供的資訊，並將驗證導向您的 AD FS STS。  在 AD FS 中註冊為公用用戶端的單一頁面應用程式，會自動設定為隱含授與流程。 授權要求會產生一個識別碼權杖，其會透過 #fragment 傳回給應用程式。 後續對後端 WebAPI 的呼叫會將此識別碼權杖當做標頭中的持有人權杖，以取得 WebAPI 的存取權。

## <a name="setting-up-the-development-box"></a>設定開發箱
本逐步解說會使用 Visual Studio 2015。 專案會使用 ADAL JS 程式庫。 若要瞭解 ADAL，請參閱[Active Directory 驗證程式庫 .net。](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>設定環境
在此逐步解說中，我們將使用的基本設定：

1.  DC：將主控 AD FS 之網域的網域控制站
2.  AD FS 伺服器：網域的 AD FS 伺服器
3.  開發電腦：我們已安裝 Visual Studio 的電腦，並將會開發我們的範例

如有需要，您可以只使用兩部電腦。 一個用於 DC/AD FS，另一個用於開發範例。

如何設定網域控制站和 AD FS 已超出本文的範圍。 如需其他部署資訊，請參閱：

- [AD DS 部署](../../ad-ds/deploy/AD-DS-Deployment.md)
- [AD FS 部署](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>複製或下載此存放庫
我們將使用為了將 Azure AD 整合到 AngularJS 單頁應用程式中所建立的範例應用程式，並將它修改為使用 AD FS 來保護後端資源。

從您的 shell 或命令列：

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>關於程式碼
包含驗證邏輯的金鑰檔如下所示：

**Node.js** -插入 adal 模組相依性、提供 adal 用來驅動與 AAD 之通訊協定互動的應用程式設定值，並指出在沒有先前驗證的情況下，不應存取哪些路由。

**index .html** -包含 adal 的參考

**HomeController**-顯示如何利用 ADAL 中的 login （）和登出（）方法。

**UserDataController** ：顯示如何從快取的 id_token 中解壓縮使用者資訊。

**Startup.Auth.cs** -包含 WebAPI 的設定，以使用 Active Directory 持有人驗證的同盟服務。

## <a name="registering-the-public-client-in-ad-fs"></a>在 AD FS 中註冊公用用戶端
在範例中，會將 WebAPI 設定為在 https://localhost:44326/接聽。 **存取 web 應用程式**的應用程式群組網頁瀏覽器可以用來設定隱含授與流程應用程式。

1. 開啟 AD FS 管理主控台，然後按一下 [**新增應用程式群組**]。 在 [**新增應用程式組嚮導]** 中，輸入應用程式的名稱、描述，然後從 [**用戶端-伺服器應用程式**] 區段中選取 [**存取 web 應用程式] 範本的網頁瀏覽器**，如下所示

    ![建立新的應用程式群組](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. 在下一個 [**原生應用程式**] 頁面上，提供應用程式用戶端識別碼和重新導向 URI，如下所示

    ![建立新的應用程式群組](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. 在下一個頁面上套用**存取控制原則**將許可權保留為 [*允許每個人*]

4. [摘要] 頁面看起來應該如下所示

    ![建立新的應用程式群組](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. 按一下 **[下一步]** 完成應用程式群組的加入，然後關閉嚮導。

## <a name="modifying-the-sample"></a>修改範例
設定 ADAL JS

開啟**應用程式 .js**檔案，並將**adalProvider**定義變更為：

    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
        );

|組態|描述|
|--------|--------|
|執行個體|您的 STS URL，例如 https://fs.contoso.com/|
|tenant|將其保留為 ' adfs '|
|clientID|這是您在設定單一頁面應用程式的公用用戶端時所指定的用戶端識別碼。|

## <a name="configure-webapi-to-use-ad-fs"></a>將 WebAPI 設定為使用 AD FS
開啟範例中的**Startup.Auth.cs**檔案，並在開頭新增下列內容：

    using System.IdentityModel.Tokens;

取消

                app.UseWindowsAzureActiveDirectoryBearerAuthentication(
    new WindowsAzureActiveDirectoryBearerAuthenticationOptions
    {
    Audience = ConfigurationManager.AppSettings["ida:Audience"],
    Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
    });

並新增：

    app.UseActiveDirectoryFederationServicesBearerAuthentication(
    new ActiveDirectoryFederationServicesBearerAuthenticationOptions
    {
    MetadataEndpoint = ConfigurationManager.AppSettings["ida:AdfsMetadataEndpoint"],
    TokenValidationParameters = new TokenValidationParameters()
    {
    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"],
    ValidIssuer = ConfigurationManager.AppSettings["ida:Issuer"]
    }
    }
    );

|參數|描述|
|--------|--------|
|ValidAudience|這會設定將在權杖中進行檢查的「物件」的值|
|ValidIssuer|這會設定將在權杖中檢查的「簽發者」值|
|MetadataEndpoint|這會指向您 STS 的中繼資料資訊|

## <a name="add-application-configuration-for-ad-fs"></a>新增 AD FS 的應用程式設定
變更 appsettings，如下所示：

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>正在執行解決方案
清除方案，重建解決方案並加以執行。 如果您想要查看詳細的追蹤，請啟動 Fiddler，並啟用 HTTPS 解密。

瀏覽器（使用 Chrome 瀏覽器）會載入 SPA，而您會看到下列畫面：

![註冊用戶端](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

按一下 [登入]。  待辦事項清單會觸發驗證流程，而 ADAL JS 會將驗證導向至 AD FS

![登入](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

在 Fiddler 中，您可以看到權杖會在 # 片段中當做 URL 的一部分傳回。

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

您現在將能夠呼叫後端 API，為已登入的使用者新增 ToDo 清單專案：

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
