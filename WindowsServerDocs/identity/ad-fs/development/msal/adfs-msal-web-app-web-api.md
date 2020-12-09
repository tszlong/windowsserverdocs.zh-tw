---
title: AD FS MSAL Web 應用程式 (server 應用程式) 呼叫 web Api
description: 瞭解如何建立 web 應用程式登入使用者，並由 AD FS 2019 進行驗證。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.openlocfilehash: ab4ec06fdf0af2c3d88a1404255fe697a4d45d8a
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866367"
---
# <a name="scenario-web-app-server-app-calling-web-api"></a>案例： Web 應用程式 (Server 應用程式) 呼叫 Web API
>適用于： AD FS 2019 和更新版本

瞭解如何建立 web 應用程式登入 2019 AD FS 使用者，並使用 [MSAL 程式庫](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) 來取得權杖，以呼叫 web api。

閱讀本文之前，您應該先熟悉 [AD FS 概念](../ad-fs-openid-connect-oauth-concepts.md) 與 [授權碼授與流程](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)

## <a name="overview"></a>概觀

![Web 應用程式呼叫 web api 的總覽](media/adfs-msal-web-app-web-api/webapp1.png)

在此流程中，您會將驗證新增至 Web 應用程式， (Server 應用程式) ，因此可讓使用者登入並呼叫 Web API。 從 Web 應用程式呼叫 Web API，使用 MSAL 的 [AcquireTokenByAuthorizationCode](/dotnet/api/microsoft.identity.client.acquiretokenbyauthorizationcodeparameterbuilder) token 取得方法。 您將使用授權碼流程，將取得的權杖儲存在權杖快取中。 然後，控制器會在需要時以無訊息方式從快取中取得權杖。 MSAL 會視需要重新整理權杖。

呼叫 Web Api 的 Web Apps：


- 是機密用戶端應用程式。
- 這就是為什麼他們將秘密註冊 (應用程式共用密碼、憑證或 AD 帳戶) AD FS。 此密碼會在呼叫 AD FS 期間傳入，以取得權杖。

若要深入瞭解如何在 ADFS 中註冊 Web 應用程式，並將它設定為取得權杖來呼叫 Web API，讓我們使用 [這裡](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi) 提供的範例，並逐步解說應用程式註冊和程式碼設定步驟。


## <a name="pre-requisites"></a>必要條件

- GitHub 用戶端工具
- AD FS 2019 或更新版本已設定並執行
- Visual Studio 2013 或更新版本

## <a name="app-registration-in-ad-fs"></a>AD FS 中的應用程式註冊
本節說明如何將 Web 應用程式註冊為機密用戶端和 Web API，作為 AD FS 中的信賴憑證者 (RP) 。

  1. 在 AD FS 管理] 中，以滑鼠右鍵按一下 [ **應用程式群組** ]，然後選取 [ **新增應用程式群組**]。
  2. 在 [應用程式群組] 嚮導中，針對 [ **名稱** ] 輸入 **WebAppToWebApi** ，然後在 [ **用戶端-伺服器應用程式** ] 下，選取 **存取 Web API 範本的伺服器應用程式** 。 按一下 [下一步] 。

      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp2.png)

  3. 複製 **用戶端識別碼** 值。 稍後將用來作為應用程式 **Web.config** 檔中 **ida： ClientId** 的值。 針對 [重新導向 URI] 輸入下列 **內容：**  -  https://localhost:44326 。 按一下 [新增]。 按一下 [下一步] 。

      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp3.png)

  4. 在 [設定應用程式認證] 畫面上，勾選 [ **產生共用密碼** ] 並複製秘密。 稍後將用來作為應用程式 **Web.config** 檔中 **ida： ClientSecret** 的值。 按一下 [下一步] 。

      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp4.png)

  5. 在 [設定 Web API] 畫面上，輸入 [ **識別碼：** ] https://webapi 。 按一下 [新增]  。 按一下 [下一步] 。 稍後在應用程式 **Web.config** 檔中，將會使用此值來進行 **ida： GraphResourceId** 。

      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp5.png)

  6. 在 [套用存取控制原則] 畫面上，選取 [**允許** 所有人]，然後按 **[下一步**

      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp6.png)

  7. 在 [設定應用程式許可權] 畫面上，確定已選取 **openid** 和 **user_impersonation** ，然後按 **[下一步]**。

      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp7.png)

  8. 在 [摘要] 畫面上，按 **[下一步]**。

  9. 在 [完成] 畫面上，按一下 [ **關閉**]。



## <a name="code-configuration"></a>程式碼設定

本節說明如何將 ASP.NET Web 應用程式設定為登入使用者，並取得權杖以呼叫 Web API

  1. 從[這裡](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi)下載範例

  2. 使用 Visual Studio 開啟範例

  3. 開啟 web.config 檔案。 修改下列各項：
       - ida： ClientId-在上述 AD FS 區段中，輸入應用程式註冊 #3 中的 **用戶端識別碼** 值。
       - ida： ClientSecret-在上述 AD FS 區段中，輸入應用程式註冊 #4 中的 **秘密** 值。
       - ida： RedirectUri-在上述 AD FS 區段中，輸入應用程式註冊 #3 中的重新 **導向 URI** 值。
       - ida：授權單位輸入 HTTPs：//[您的 AD FS 主機名稱]/adfs。 例如，https://adfs.contoso.com/adfs
       - ida：資源-在上述 AD FS 區段中，輸入應用程式註冊 #5 中的 **識別碼** 值。

          ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp8.png)


### <a name="test-the-sample"></a>測試範例
本節說明如何測試上述設定的範例。

  1. 一旦變更程式碼之後，就會重新建立解決方案

  2. 在 Visual Studio 的頂端，確定已選取 Internet Explorer，然後按一下綠色箭號。

      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp9.png)

  3. 在首頁上，按一下 [登入]。

      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp10.png)

  4. 系統會將您重新導向至 AD FS 登入頁面。 繼續進行並登入。

      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp11.png)

  5. 登入後，按一下 [存取權杖]。

      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp12.png)

  6. 按一下存取權杖將會藉由呼叫 Web API 來取得存取權杖資訊

      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp13.png)

 ## <a name="next-steps"></a>後續步驟
[AD FS OpenID Connect/OAuth 流程和應用程式案例](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)

