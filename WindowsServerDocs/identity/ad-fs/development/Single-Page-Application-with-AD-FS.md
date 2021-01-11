---
title: 使用 OAuth 和 ADAL.JS 搭配 AD FS 2016 或更新版本來建立單一頁面 web 應用程式
description: 本逐步解說提供使用 ADAL for JavaScript 保護 AngularJS 型單一頁面應用程式，以針對 AD FS 進行驗證的指示。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/13/2018
ms.topic: article
ms.openlocfilehash: 3c476de50a10d48757f19c41c80f10411222d885
ms.sourcegitcommit: 29b8942ea46196c12a67f6b6ad7f8dd46bf94fb2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2021
ms.locfileid: "98065654"
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016-or-later"></a>使用 OAuth 和 ADAL.JS 搭配 AD FS 2016 或更新版本來建立單一頁面 web 應用程式

本逐步解說提供的指示可讓您使用適用于 JavaScript 的 AngularJS 來驗證 AD FS，以保護以 ASP.NET Web API 後端執行的單一頁面應用程式。

在此案例中，當使用者登入時，JavaScript 前端會使用 [Active Directory Authentication Library for JavaScript (ADAL.JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) 和隱含授權授與，從 Azure AD 取得的識別碼權杖 (id_token)。 權杖會留在快取中，而用戶端呼叫 Web API 後端時會將權杖附加至要求做為持有人權杖，並使用 OWIN 中介軟體來保護。

> [!IMPORTANT]
> 您可以在這裡建立的範例僅供教育之用。 這些指示是為了公開模型的必要元素，最簡單且最基本的實作為可能。 此範例可能不包含錯誤處理和其他相關功能的所有層面。

> [!NOTE]
> 本逐步解說 **僅** 適用于 AD FS Server 2016 和更新版本

## <a name="overview"></a>概觀
在此範例中，我們將建立驗證流程，其中單一頁面應用程式用戶端會向 AD FS 驗證，以安全地存取後端上的 WebAPI 資源。 以下是整體驗證流程

![AD FS 授權](media/Single-Page-Application-with-AD-FS/authenticationflow.png)

使用單一頁面應用程式時，使用者會流覽至起始位置，從這裡開始的頁面和 JavaScript 檔案的集合，以及 HTML 視圖都已載入。 您必須設定 Active Directory 驗證程式庫 (ADAL) 來瞭解應用程式的重要資訊，例如 AD FS 實例、用戶端識別碼，讓它可以將驗證導向您的 AD FS。

如果 ADAL 看到驗證的觸發程式，則會使用應用程式所提供的資訊，並將驗證導向您的 AD FS STS。  單一頁面應用程式（註冊為 AD FS 中的公用用戶端）會自動針對隱含授與流程進行設定。 授權要求會產生透過 #fragment 傳回給應用程式的識別碼權杖。 對後端 WebAPI 的後續呼叫會將此識別碼權杖作為標頭中的持有人權杖，以取得 WebAPI 的存取權。

## <a name="setting-up-the-development-box"></a>設定開發箱
本逐步解說使用 Visual Studio 2015。 專案使用 ADAL JS 程式庫。 若要瞭解有關 ADAL 的詳細資訊，請參閱 [Active Directory 驗證程式庫 .net。](/dotnet/api/microsoft.identitymodel.clients.activedirectory)

## <a name="setting-up-the-environment"></a>設定環境
在這個逐步解說中，我們將使用的基本設定：

1. DC：將裝載 AD FS 之網域的網域控制站
2. AD FS 伺服器：網域的 AD FS 伺服器
3. 開發電腦：已安裝 Visual Studio 的電腦，將會開發我們的範例

您可以視需要使用兩部電腦。 一個用於 DC/AD FS，另一個用於開發範例。

如何設定網域控制站和 AD FS 已超出本文的範圍。 如需其他部署資訊，請參閱：

- [AD DS 部署](../../ad-ds/deploy/AD-DS-Deployment.md)
- [AD FS 部署](../AD-FS-Deployment.md)

## <a name="clone-or-download-this-repository"></a>複製或下載此存放庫
我們將使用針對將 Azure AD 整合至 AngularJS 單一頁面應用程式中所建立的範例應用程式，並使用 AD FS 來修改後端資源，以保護後端資源。

從殼層或命令列：

```
git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git
```

## <a name="about-the-code"></a>關於程式碼
包含驗證邏輯的金鑰檔如下：

**App.js** -插入 adal 模組相依性，提供 adal 用來驅動與 AAD 互動之通訊協定的應用程式設定值，並指出不應在先前的驗證下存取哪些路由。

**index.html** -包含 adal.js 的參考

**HomeController.js**-示範如何在 ADAL 中利用登入 ( # A1 和登出 ( # A3 方法。

**UserDataController.js** -示範如何從快取的 id_token 中解壓縮使用者資訊。

**Startup.Auth.cs** -包含 WebAPI 的設定，可使用 Active Directory 同盟服務進行持有者驗證。

## <a name="registering-the-public-client-in-ad-fs"></a>在 AD FS 中註冊公用用戶端
在此範例中，WebAPI 設定為接聽 https://localhost:44326/ 。 **存取 web 應用程式** 的應用程式群組網頁瀏覽器可以用來設定隱含授與流程應用程式。

1. 開啟 AD FS 管理主控台，然後按一下 [ **新增應用程式群組**]。 在 [**新增應用程式群組]** 中，輸入應用程式的名稱、描述，然後從 [**用戶端-伺服器應用程式**] 區段中選取要 **存取 Web 應用程式範本的網頁瀏覽器**，如下所示

    ![建立新的應用程式群組](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. 在下一個頁面的 **原生應用程式** 上，提供應用程式用戶端識別碼和重新導向 URI，如下所示

    ![建立新的應用程式群組](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. 在下一個頁面上，套用 **存取控制原則**，讓 *所有人都* 能擁有

4. 摘要頁面看起來應該如下所示

    ![建立新的應用程式群組](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. 按一下 **[下一步** ]，完成應用程式群組的新增，然後關閉嚮導。

## <a name="modifying-the-sample"></a>修改範例
設定 ADAL JS

開啟 **app.js** 檔，並將 **adalProvider.init** 定義變更為：

```
    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
    );
```

| 組態 | 描述 |
| ------------- | ----------- |
| instance | 您的 STS URL，例如 https://fs.contoso.com/ |
| tenant | 將其保留為 ' adfs ' |
| clientID | 這是您為單一頁面應用程式設定公用用戶端時所指定的用戶端識別碼。 |

## <a name="configure-webapi-to-use-ad-fs"></a>將 WebAPI 設定為使用 AD FS
在範例中開啟 **Startup.Auth.cs** 檔案，並在開頭新增下列內容：

```
    using System.IdentityModel.Tokens;

remove:

    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        }
    );

and add:

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
```

| 參數 | 說明 |
| --------- | ----------- |
| ValidAudience | 這會設定將在權杖中檢查的「物件」值 |
| ValidIssuer | 這會設定將在權杖中檢查的「簽發者」值 |
| MetadataEndpoint | 這會指向 STS 的中繼資料資訊 |

## <a name="add-application-configuration-for-ad-fs"></a>新增 AD FS 的應用程式設定
變更 appSettings XML，如下所示：

```xml
<appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
</appSettings>
```

## <a name="running-the-solution"></a>正在執行方案
清除解決方案、重建解決方案，然後執行它。 如果您想要查看詳細追蹤，請啟動 Fiddler 並啟用 HTTPS 解密。

瀏覽器 (使用 Chrome 瀏覽器) 將載入 SPA，而且您將會看到下列畫面：

![註冊用戶端](media/Single-Page-Application-with-AD-FS/singleapp3.png)

按一下 [登入]。  ToDo 清單會觸發驗證流程，而 ADAL JS 會將驗證導向 AD FS

![登入](media/Single-Page-Application-with-AD-FS/singleapp4a.png)

在 Fiddler 中，您可以在 # 片段中看到傳回的權杖是 URL 的一部分。

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.png)

您現在可以呼叫後端 API，為登入的使用者新增 ToDo 清單專案：

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.png)

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)
