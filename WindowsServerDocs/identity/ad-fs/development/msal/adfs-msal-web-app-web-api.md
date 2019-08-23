---
title: AD FS MSAL Web 應用程式 (伺服器應用程式) 呼叫 web Api
description: 瞭解如何建立由 AD FS 2019 驗證的 web 應用程式登入使用者。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d2ac36180992d44f837ce74ace40cf95533309c9
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2019
ms.locfileid: "69983426"
---
# <a name="scenario-web-app-server-app-calling-web-api"></a>案例：呼叫 Web API 的 web 應用程式 (伺服器應用程式) 
>適用於：AD FS 2019 和更新版本 
 
瞭解如何建立 web 應用程式登入 AD FS 2019 驗證的使用者, 並使用[MSAL 程式庫](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)來呼叫 web api 來取得權杖。  
 
在閱讀本文之前, 您應該先熟悉[AD FS 概念](../ad-fs-openid-connect-oauth-concepts.md)和[授權碼授與流程](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)
 
## <a name="overview"></a>總覽 
 
![Web 應用程式呼叫 web api 的總覽](media/adfs-msal-web-app-web-api/webapp1.png)

在此流程中, 您會將驗證新增至您的 Web 應用程式 (伺服器應用程式), 進而登入使用者並呼叫 Web API。 從 Web 應用程式, 若要呼叫 Web API, 請使用 MSAL 的[AcquireTokenByAuthorizationCode](https://docs.microsoft.com/en-us/dotnet/api/microsoft.identity.client.acquiretokenbyauthorizationcodeparameterbuilder?view=azure-dotnet) token 取得方法。 您將使用授權碼流程, 將取得的權杖儲存在權杖快取中。 然後, 控制器會在需要時, 以無訊息方式從快取中取得權杖。 MSAL 會視需要重新整理權杖。 

呼叫 Web Api 的 Web Apps: 


- 是機密用戶端應用程式。 
- 這就是為什麼他們已使用 AD FS 來註冊秘密 (應用程式共用密碼、憑證或 AD 帳戶)。 此密碼會在呼叫 AD FS 時傳入, 以取得權杖。  

若要進一步瞭解如何在 ADFS 中註冊 Web 應用程式, 並將它設定為取得權杖以呼叫 Web API, 讓我們使用[這裡](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi)提供的範例, 並逐步解說應用程式註冊和程式碼設定步驟。  

 
## <a name="pre-requisites"></a>先決條件 

- GitHub 用戶端工具 
- 已設定且正在執行 AD FS 2019 或更新版本 
- Visual Studio 2013 或更新版本 
 
## <a name="app-registration-in-ad-fs"></a>AD FS 中的應用程式註冊 
本節說明如何在 AD FS 中, 將 Web 應用程式註冊為機密用戶端和 Web API 做為信賴憑證者 (RP)。 

  1. 在 AD FS 管理 中, 以滑鼠右鍵按一下 **應用程式群組**, 然後選取 **新增應用程式群組**。  
  2. 在 [應用程式組嚮導] 的 [**名稱**] 中, 輸入**WebAppToWebApi** , 然後在 [**用戶端-伺服器應用程式**] 下, 選取**存取 Web API 範本的伺服器應用程式**。 按一下 [下一步]。  
  
      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp2.png)
  
  3. 複製 [**用戶端識別碼**] 值。 稍後在應用程式的 web.config 檔案中, 將會使用它做為 **ida: ClientId**的值。 針對 [重新導向 URI] 輸入下列**內容:**  -  https://localhost:44326 。 按一下 [新增]。 按一下 [下一步]。 
  
      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp3.png)
  
  4. 在 [設定應用程式認證] 畫面上, 勾選 [**產生共用密碼**] 和 [複製密碼]。 稍後在應用程式的 web.config 檔案中, 這將會用來當做 **ida: ClientSecret**的值。 按一下 [下一步]。  
  
      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp4.png)
  
  5. 在 [設定 Web API] 畫面上, 輸入**識別碼:** https://webapi 。 按一下 [新增]。 按一下 [下一步]。 稍後在應用程式的 web.config 檔案中, 將會使用此值進行**ida: GraphResourceId** 。 
  
      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp5.png)
  
  6. 在 [套用存取控制原則] 畫面上選取 [**允許**每個人], 然後按 **[下一步]** 
  
      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp6.png)
  
  7. 在 [設定應用程式許可權] 畫面上, 確認已選取**openid**和**user_impersonation** , 然後按 **[下一步]** 。 
  
      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp7.png)
  
  8. 在 [摘要] 畫面上, 按 **[下一步]** 。 
  
  9. 在 [完成] 畫面上, 按一下 [**關閉**]。



## <a name="code-configuration"></a>程式碼設定 

本節說明如何將 ASP.NET Web 應用程式設定為登入使用者, 並取得權杖以呼叫 Web API 

  1. 從[這裡](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi)下載範例   
  
  2. 使用 Visual Studio 開啟範例 
  
  3. 開啟 web.config 檔案。 修改下列各項: 
       - ida: ClientId-在上述 AD FS 區段中, 輸入應用程式註冊 #3 的 [**用戶端識別碼**] 值。 
       - ida: ClientSecret-在上述 AD FS 區段中, 從應用程式註冊 #4 輸入**秘密**值。 
       - ida: RedirectUri-在上述 AD FS 區段中, 輸入應用程式註冊 #3 的 [重新**導向 URI** ] 值。 
       - ida: 授權單位-輸入 HTTPs://[您的 AD FS hostname]/adfs。 例如, https://adfs.contoso.com/adfs 
       - ida: 資源-在上述 AD FS 區段中, 輸入應用程式註冊 #5 的**識別碼**值。 
      
          ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp8.png)
 
 
### <a name="test-the-sample"></a>測試範例 
本節說明如何測試上述設定的範例。 

  1. 程式碼變更之後, 請重新建立解決方案 
  
  2. 在 [Visual Studio] 頂端, 確定已選取 [Internet Explorer], 然後按一下綠色箭號。 
  
      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp9.png)

  3. 在 [首頁] 上, 按一下 [登入]。 
  
      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp10.png)

  4. 系統會將您重新導向至 AD FS 登入頁面。 請繼續並登入。 
  
      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp11.png)

  5. 登入後, 按一下 [存取權杖]。  
  
      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp12.png)

  6. 按一下存取權杖將會藉由呼叫 Web API 來取得存取權杖資訊 
  
      ![新增應用程式群組](media/adfs-msal-web-app-web-api/webapp13.png)
 
 ## <a name="next-steps"></a>後續步驟
[AD FS OpenID Connect/OAuth 流程和應用程式案例](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 