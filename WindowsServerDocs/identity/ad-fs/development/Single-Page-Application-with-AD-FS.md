---
title: 建置使用 OAuth 和 ADAL 的單一頁面 web 應用程式。JS 與 AD FS 2016 或更新版本
description: 逐步解說中提供對 AD FS 進行驗證的指示，使用 ADAL for JavaScript 保護 AngularJS 架構的單一頁面應用程式
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 1292c7e6cd1dec6926516880c34fe60fb97a9ec8
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190496"
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016-or-later"></a>建置使用 OAuth 和 ADAL 的單一頁面 web 應用程式。JS 與 AD FS 2016 或更新版本

本逐步解說會提供指示，對 AD FS 保護 AngularJS javascript 中使用 ADAL 進行驗證以單一頁面應用程式，使用 ASP.NET Web API 後端實作。

在此案例中，當使用者登入時，JavaScript 前端會使用[Active Directory Authentication Library for JavaScript (ADAL。JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js)和隱含授權授與從 Azure AD 取得 ID 權杖 (id_token)。 在快取權杖，用戶端將它附加至要求做為持有人權杖呼叫 Web API 後端，並使用 OWIN 中介軟體保護時。

>警告：您可以在這裡建置的範例是僅供教育。 這些指示是最簡單、 最小實作能夠公開 （expose） 模型的必要項目。 範例可能不會包含錯誤處理的所有層面，以及其他與關聯的功能。

>[!NOTE]
>這個逐步解說是適用於**只**2016年和更新版本的 AD FS 伺服器 

## <a name="overview"></a>總覽
在此範例中，我們將建立單一頁面應用程式用戶端驗證與 AD FS 來保護後端上的 WebAPI 資源的存取權的其中一個驗證流程。 以下是整體的驗證流程


![AD FS 授權](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

使用時的單一頁面應用程式，使用者瀏覽到起始的位置，從在起始頁面和 JavaScript 檔案的集合並載入 HTML 檢視。 您需要設定 Active Directory Authentication Library (ADAL) 知道您的應用程式，也就是 AD FS 執行個體，用戶端識別碼和相關的重要資訊，讓它可導向至您的 AD FS 驗證。

如果 ADAL 進行驗證的觸發程序，它會使用應用程式所提供的資訊，並將導向至您的 AD FS STS 驗證。  單一頁面應用程式，註冊為公用用戶端在 AD FS 中，會自動設定隱含授與流程。 授權要求會導致透過 #fragment 傳回到應用程式的識別碼權杖。 進一步的呼叫後端 WebAPI 會做為持有人權杖來存取 WebAPI 的標頭中包含此識別碼權杖。

## <a name="setting-up-the-development-box"></a>設定開發電腦
此逐步解說會使用 Visual Studio 2015。 專案會使用 ADAL JS 程式庫。 若要了解 ADAL，請閱讀[Active Directory Authentication Library.NET。](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>設定環境
此逐步解說中，我們將使用的基本安裝：

1.  DC:將在其中裝載 AD FS 的網域的網域控制站
2.  AD FS 伺服器：網域的 AD FS 伺服器
3.  開發電腦：我們已安裝 Visual Studio 和開發我們的範例中的機器

您可以如果您想，使用只有兩部機器。 一個用於 DC/AD FS 與另一個則用於開發的範例。

如何設定網域控制站和 AD FS 已超出本文的範圍。 其他部署資訊，請參閱：

- [AD DS 部署](../../ad-ds/deploy/AD-DS-Deployment.md) 
- [AD FS 部署](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>複製或下載此存放庫
我們將改為使用 AD FS 保護的後端資源使用範例應用程式建立的 Azure AD 整合的 AngularJS 單一頁面應用程式，並修改它。

從您的殼層或命令列：

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>關於程式碼
包含驗證邏輯的金鑰檔案如下所示：

**App.js** -插入 ADAL 模組相依性，提供推動與 AAD 通訊協定互動使用 ADAL 的應用程式組態值和指出哪些路由不應該存取沒有先前的驗證。

**index.html** -包含 adal.js 的參考

**HomeController.js**-示範如何利用 adal 的 login （） 和 logOut() 方法。

**UserDataController.js** -示範如何從快取的 id_token 擷取使用者資訊。

**Startup.Auth.cs** -包含要用於持有人驗證中的 Active Directory Federation Service WebAPI 的組態。

## <a name="registering-the-public-client-in-ad-fs"></a>在 AD FS 中註冊公開用戶端
在此範例中，WebAPI 會設定為接聽 https://localhost:44326/。 應用程式群組**存取的 web 應用程式的網頁瀏覽器**可用來設定隱含授與流程應用程式。

1. 開啟 AD FS 管理主控台，然後按一下**加入應用程式群組**。 中**新增應用程式群組精靈**輸入名稱的應用程式、 描述，然後選取**存取的 web 應用程式的網頁瀏覽器**從範本**用戶端-伺服器應用程式**區段，如下所示  <br>![建立新的應用程式群組](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. 在下一頁**原生應用程式**、 提供的應用程式用戶端識別碼和重新導向 URI，如下所示  <br>![建立新的應用程式群組](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. 在下一頁**套用存取控制原則**保留的權限與*允許所有人*

4. [摘要] 頁面看起來應該類似下方所示  <br>![建立新的應用程式群組](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. 按一下 [**下一步]** 完成的應用程式群組，並關閉精靈。

## <a name="modifying-the-sample"></a>修改範例
設定 ADAL JS

開啟**app.js**檔案，並變更**adalProvider.init**定義：

    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
        );

|組態|描述
|--------|--------
|執行個體|您 STS 的 URL，例如 https://fs.contoso.com/
|租用戶|將其保留為 'adfs'
|clientID|這是設定為單一頁面應用程式公開的用戶端時指定的用戶端識別碼

## <a name="configure-webapi-to-use-ad-fs"></a>設定為使用 AD FS 的 WebAPI
開啟**Startup.Auth.cs**範例檔案，並在開頭新增下列： 

    using System.IdentityModel.Tokens;

移除：

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

|參數|描述
|--------|--------
|ValidAudience|這會設定 'audience' 的值，將會檢查對權杖中
|ValidIssuer|這會設定的值 ' 將會針對簽入語彙基元的簽發者
|MetadataEndpoint|這會指向您的 STS 的中繼資料資訊

## <a name="add-application-configuration-for-ad-fs"></a>新增適用於 AD FS 的應用程式設定
變更為 appsettings，如下所示：

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>執行解決方案
清除方案，重新建置方案並予以執行。 如果您想要查看其詳細的追蹤，請啟動 Fiddler，並啟用 HTTPS 解密。

瀏覽器 （使用 Chrome 瀏覽器） 會載入的 SPA，您會看到下列畫面：

![註冊用戶端](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

按一下 登入。  待辦事項清單將會觸發驗證流程並 ADAL JS 會導向至 AD FS 驗證

![登入](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

在 Fiddler 中，您可以看到正在 # 片段中的 URL 的一部分傳回的語彙基元。

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

您將能夠立即呼叫後端 API 新增登入的使用者的待辦事項清單項目：

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
