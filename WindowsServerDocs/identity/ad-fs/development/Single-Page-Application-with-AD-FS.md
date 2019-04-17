---
title: "組建使用 OAuth 和 ADAL.JS"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 7b3d48e1e38baffeb84b1f236efb43cfda5048c0
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016"></a>組建使用 OAuth 和 ADAL.JS

>適用於：Windows Server 2016

本節提供驗證 AD FS 使用 ADAL 適用於 JavaScript 保護 AngularJS 指令單頁應用程式、 ASP.NET Web API 端與實作。

>警告：僅供教育是您可以在此組建的範例。 這些指示僅適用於可能公開所需項目模式的最簡單、最小實作。 範例可能不會包含錯誤處理的一切和其他相關功能。

>[!NOTE]
>本節適**只**以 AD FS Server 2016 和更高版本 

## <a name="overview"></a>概觀
在此範例中，我們將建立單頁應用程式 client 位置將會驗證 AD FS 保護的存取權端 WebAPI 資源驗證流程。 以下是整體驗證流程


![AD FS 授權](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

時使用單頁應用程式時，使用者瀏覽至開始的位置，從開始網頁和一系列 JavaScript 檔案，並會載入 HTML 檢視。 您需要設定 Active Directory 驗證媒體櫃 (ADAL) 若要知道您的應用程式，也就是 AD FS 執行個體 client ID 的重要資訊，讓它可以直接驗證您 AD FS。

如果 ADAL 看到觸發程序進行驗證時，它會使用應用程式所提供的資訊，並會引導您 AD FS STS 驗證。  AD FS 在公用 client 為登記完畢單頁應用程式會自動設定隱含授與流程。 要求授權會導致 ID 權杖透過 #fragment 傳回回應用程式。 進一步來電後端 WebAPI 將承載權杖標題存取 WebAPI 中執行此 ID 預付碼。

## <a name="setting-up-the-development-box"></a>開發方塊設定
這個解說使用 Visual Studio 2015。 專案使用 ADAL JS 媒體櫃。 若要深入了解 ADAL 請朗讀[Active Directory 驗證庫.NET。](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>設定環境
適用於本節我們將會使用基本的安裝：

1.  AD FS 可裝載網域俠： 網域控制站
2.  AD FS: AD FS 伺服器的網域
3.  我們已安裝 Visual Studio，將會開發我們範例開發電腦： 電腦

您可以如果您想要使用只兩部電腦。 另一個用於俠日 AD FS，另一個用於範例開發無障礙。

如何設定的網域控制站和 AD FS 超出範圍的這篇文章。 適用於其他部署的資訊查看：

- [AD DS 部署](../../ad-ds/deploy/AD-DS-Deployment.md) 
- [AD FS 部署](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>複製或下載這個存放庫
我們會使用 Azure AD 整合 AngularJS 單頁應用程式與它修改建立的範例應用程式，改為使用 AD FS 保護端資源。

從命令列或殼層：

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>程式碼
包含驗證邏輯金鑰檔案如下所示：

**App.js** -ADAL 模組相依性會將提供的應用程式的設定值 ADAL 用於 「 駕駛通訊協定 AAD 互動，表示的路徑不應該存取而先前的驗證。

**index.html** -包含 adal.js 參考

**HomeController.js**-示範如何 ADAL 利用 login() 和 logOut() 的方法。

**UserDataController.js**的顯示快取 id_token 從解壓縮使用者資訊的方式。

**Startup.Auth.cs** -包含承載驗證使用 Active Directory 同盟服務 WebAPI 設定。

## <a name="registering-the-public-client-in-ad-fs"></a>AD FS 中登記公用 client
在此範例中，若要聆聽，https://localhost:44326 設定 WebAPI 日。 應用程式群組**存取 web 應用程式的網頁瀏覽器**可用於設定隱含授與流程應用程式。

1. 打開 AD FS 管理主控台中，並按一下**[新增應用程式群組**。 在**的應用程式群組精靈**輸入名稱的應用程式，描述並選取 [**存取 web 應用程式的網頁瀏覽器**從範本**Client 伺服器應用程式**區段，如下所示
    <br>![建立新的應用程式群組](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. 在下一個頁面上**應用程式原生**提供應用程式 client 識別碼，如下所示，重新導向 URI
    <br>![建立新的應用程式群組](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. 在下一個頁面上**適用於存取控制原則**離開權限*讓任何人*

4. 看起來類似下列摘要頁面
    <br>![建立新的應用程式群組](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. 按一下**下一步**以完成的應用程式群組，並關閉精靈。

## <a name="modifying-the-sample"></a>修改範例
設定 ADAL JS

開放**app.js**檔案，然後變更**adalProvider.init**來定義：

    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
        );

|設定|描述
|--------|--------
|執行個體|您 STS 的 URL，例如 https://fs.contoso.com/
|承租人|將其保留為 'adfs'
|clientID|這是您指定的時設定的應用程式單頁公用 client client ID

## <a name="configure-webapi-to-use-ad-fs"></a>設定使用 AD FS WebAPI
開放**Startup.Auth.cs**在此範例中的檔案，然後新增下列開頭︰ 

    using System.IdentityModel.Tokens;

移除：

                app.UseWindowsAzureActiveDirectoryBearerAuthentication(
    new WindowsAzureActiveDirectoryBearerAuthenticationOptions
    {
    Audience = ConfigurationManager.AppSettings["ida:Audience"],
    Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
    });

並加入：

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
|ValidAudience|這會設定為 '對象'，將會針對檢查權杖中
|ValidIssuer|這會設定為 ' 會針對權杖中檢查的發行者
|MetadataEndpoint|這指向您 STS 中繼資料資訊

## <a name="add-application-configuration-for-ad-fs"></a>新增 AD FS 應用程式設定
變更和 appsettings 如下：

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>執行方案
清除方案、 重建方案和執行。 如果您想要查看詳細的追蹤，上市 Fiddler 以及 HTTPS 解密。

瀏覽器將會載入] 選項，您將會看到以下畫面：

![登記 client](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

按一下 [登入。  會待辦清單會觸發驗證流程並 ADAL JS 會直接驗證，AD FS

![登入](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

在 Fiddler，您可以看到傳回 # 片段中的 URL 的一部分預付碼。

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

您可以立即來電後端的 API，若要新增的登入的使用者待辦清單項目：

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
