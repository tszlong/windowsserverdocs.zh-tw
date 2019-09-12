---
title: AD FS MSAL Web API 呼叫 Web API （代表案例）
description: 瞭解如何建立呼叫另一個 Web API 的 Web API。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2ab6141b84d03102c5dedd1ede0ba99e5adf3e4a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867752"
---
# <a name="scenario-web-api-calling-web-api-on-behalf-of-scenario"></a>案例：Web API 呼叫 Web API （代表案例） 
> 適用於：AD FS 2019 和更新版本 
 
瞭解如何代表使用者建立呼叫另一個 Web API 的 Web API。  
 
在閱讀本文之前，您應該先熟悉[AD FS 概念](../ad-fs-openid-connect-oauth-concepts.md)和[Behalf_Of 流程](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#on-behalf-of-flow)

## <a name="overview"></a>總覽 


- 用戶端（Web 應用程式）-不會在下圖中表示-呼叫受保護的 Web API，並在其「授權」 Http 標頭中提供 JWT 持有人權杖。 
- 受保護的 Web API 會驗證權杖，並使用 MSAL [AcquireTokenOnBehalfOf](https://docs.microsoft.com/en-us/dotnet/api/microsoft.identitymodel.clients.activedirectory.authenticationcontext.acquiretokenasync?view=azure-dotnet#Microsoft_IdentityModel_Clients_ActiveDirectory_AuthenticationContext_AcquireTokenAsync_System_String_Microsoft_IdentityModel_Clients_ActiveDirectory_ClientCredential_Microsoft_IdentityModel_Clients_ActiveDirectory_UserAssertion_)  方法來要求（從 AD FS）另一個權杖，使其本身可以代表使用者呼叫第二個 Web API （名為下游 Web API）。 
- 受保護的 Web API 會使用此權杖來呼叫下游 API。 它也可以呼叫 AcquireTokenSilentlater 來要求其他下游 Api （但仍代表相同的使用者）的權杖。 AcquireTokenSilent 會在需要時重新整理權杖。  
 
     ![概觀](media/adfs-msal-web-api-web-api/webapi1.png)
 
若要進一步瞭解如何在 ADFS 中代表驗證案例進行設定，讓我們使用[這裡](https://github.com/microsoft/adfs-sample-msal-dotnet-webapi-to-webapi-onbehalfof)提供的範例，並逐步解說應用程式註冊和程式碼設定步驟。  
 
## <a name="pre-requisites"></a>先決條件 

- GitHub 用戶端工具 
- 已設定且正在執行 AD FS 2019 或更新版本 
- Visual Studio 2013 或更新版本 
 
## <a name="app-registration-in-ad-fs"></a>AD FS 中的應用程式註冊 

本節說明如何在 AD FS 中，將原生應用程式註冊為公用用戶端和 Web Api 做為信賴憑證者（RP） 

  1. 在 AD FS 管理 中，以滑鼠右鍵按一下 **應用程式群組**，然後選取 **新增應用程式群組**。  
  
  2. 在 [應用程式群組] 上，針對 [**名稱**] 輸入**WebApiToWebApi** ，然後在 [**用戶端-伺服器應用程式**] 下，選取**存取 Web API 範本的原生應用程式**。 按一下 [下一步]。

      ![應用程式註冊](media/adfs-msal-web-api-web-api/webapi2.png)

  3. 複製 [**用戶端識別碼**] 值。 稍後**在應用程式的 app.config**檔案中，將會使用它做為**ClientId**的值。 針對 [重新導向 URI] 輸入下列**內容：**  -  https://ToDoListClient 。 按一下 [新增]。 按一下 [下一步]。 
  
      ![應用程式註冊](media/adfs-msal-web-api-web-api/webapi3.png)
  
  4. 在 [設定 Web API] 畫面上，輸入**識別碼：** https://localhost:44321/ 。 按一下 [新增]。 按一下 [下一步]。 稍後會在**應用程式的 app.config 和 web.config**檔案中使用此值 **。**  
 
      ![應用程式註冊](media/adfs-msal-web-api-web-api/webapi4.png)

  5. 在 [套用存取控制原則] 畫面上選取 [**允許每個人**]，然後按 **[下一步]** 
  
      ![應用程式註冊](media/adfs-msal-web-api-web-api/webapi5.png)  

  6. 在 [設定應用程式許可權] 畫面上，選取 [ **openid** and **user_impersonation**]。 按一下 [下一步]。  
  
      ![應用程式註冊](media/adfs-msal-web-api-web-api/webapi6.png)  

  7. 在 [摘要] 畫面上，按 **[下一步]** 。 

  8. 在 [完成] 畫面上，按一下 [**關閉**]。 


  9. 在 AD FS 管理 中，按一下 **應用程式群組**，然後選取  **WebApiToWebApi**應用程式群組。 按一下滑鼠右鍵並選取 [內容]。 
  
      ![應用程式註冊](media/adfs-msal-web-api-web-api/webapi7.png)  

  10. 在 [WebApiToWebApi 屬性] 畫面上，按一下 [**加入應用程式**]。 
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi8.png)

  11. 在 [獨立應用程式] 下，選取 [**伺服器應用程式**]。  
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi9.png)

  12. 在 [伺服器應用程式] https://localhost:44321/ 畫面上，加入做為**用戶端識別碼**和重新**導向 URI**。 
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi10.png)

  13. 在 [設定應用程式認證] 畫面上，選取 [**產生共用密碼**]。 複製密碼以供稍後使用。
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi11.png)

  14. 在 [摘要] 畫面上，按 **[下一步]** 。 

  15. 在 [完成] 畫面上，按一下 [**關閉**]。 

  16. 在 AD FS 管理 中，按一下 **應用程式群組**，然後選取  **WebApiToWebApi**應用程式群組。 按一下滑鼠右鍵並選取 [內容]。 
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi12.png)

  17. 在 [WebApiToWebApi 屬性] 畫面上，按一下 [**加入應用程式**]。 
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi13.png)

  18. 在 [獨立應用程式] 下，選取 [ **WEB API**]。 
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi14.png)  

  19. 在 [設定 Web API] https://localhost:44300 上，新增作為**識別碼**。  
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi15.png)

  20. 在 [套用存取控制原則] 畫面上選取 [**允許每個人**]，然後按 **[下一步]** 
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi16.png)

  21. 在 [設定應用程式許可權] 畫面上，按 **[下一步]** 。 
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi17.png)

  22. 在 [摘要] 畫面上，按 **[下一步]** 。

  23. 在 [完成] 畫面上，按一下 [**關閉**]。  

  24. 在 [WebApiToWebApi – Web API 2 屬性] 畫面上按一下 [確定]  

  25. 在 [WebApiToWebApi 屬性] 畫面上，選取 [ **WebApiToWebApi – WEB API** ]，然後按一下 [**編輯**]。  
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi18.png)

  26. 在 [WebApiToWebApi-Web API 屬性] 畫面上，選取 [**發行轉換規則**] 索引標籤，然後按一下 [**新增規則 ...** ]。 
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi19.png)

  27. 在 新增轉換宣告規則 上，從下拉式清單中選取 **使用自訂規則傳送宣告**，然後按**下一步** 
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi20.png)

  28. 在 [宣告**規則名稱：** ] 欄位中輸入**PassAllClaims** ， **x： [] = > 問題（宣告 = x）;** 自訂規則中的宣告規則：欄位，然後按一下 [完成]。  
   
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi21.png)

  29. 在 [WebApiToWebApi – Web API 屬性] 畫面上按一下 [確定]

  30. 在 [WebApiToWebApi 屬性] 畫面上，選取 [選取 WebApiToWebApi – Web API 2]，然後按一下 [編輯]。</br> 
  ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi22.png)

  31. 在 [WebApiToWebApi-Web API 2 屬性] 畫面上，選取 [發行轉換規則] 索引標籤，然後按一下 [新增規則 ...] 

  32. 在 [新增轉換宣告規則] 上，選取 [使用來自 dopdown 的自訂規則傳送![宣告]，然後按一下 [下一步應用程式]](media/adfs-msal-web-api-web-api/webapi23.png)

  33. 在 [宣告規則名稱：] 欄位中輸入 PassAllClaims， **x： [] = > 問題（宣告 = x）;** **自訂規則**中的宣告規則： 欄位，然後按一下 **[完成]** 。  
   
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi24.png)

  34.  在 [WebApiToWebApi – Web API 2 屬性] 畫面上按一下 [確定]，然後在 [WebApiToWebApi 屬性] 畫面上。  
 

## <a name="code-configuration"></a>程式碼設定 

本節說明如何設定 Web API 以呼叫另一個 Web API 

  1. 從[這裡](https://github.com/microsoft/adfs-sample-msal-dotnet-webapi-to-webapi-onbehalfof)下載範例  
  
  2. 使用 Visual Studio 開啟範例 
  
  3. 開啟 App.config 檔案。 修改下列各項： 
       - ida：授權單位-輸入 HTTPs：//[您的 AD FS hostname]/adfs/
       - ida： ClientId-在上述 AD FS 區段中，輸入應用程式註冊 #3 的值。 
       - ida： RedirectUri-在上述 AD FS 區段中，輸入應用程式註冊 #3 的值。 
       - todo： TodoListResourceId –在上方 AD FS 區段中，從應用程式註冊 #4 輸入識別碼值 
       - ida： todo： TodoListBaseAddress-在上述 AD FS 區段中，輸入應用程式註冊 #4 的識別碼值。 
      
            ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi25.png)

  4. 開啟 ToDoListService 底下的 web.config 檔案。 修改下列各項： 
       - ida：物件-在上 AD FS 區段中，從應用程式註冊 #12 輸入 [用戶端識別碼] 值
       - ida： ClientId-在上述 AD FS 區段中，輸入應用程式註冊 #12 的 [用戶端識別碼] 值。 
       - idaClientSecret-在上述 AD FS 區段中，輸入從應用程式註冊 #13 複製的共用密碼。
       - ida： RedirectUri-在上述 AD FS 區段中，輸入應用程式註冊 #12 的 RedirectUri 值。 
       - idaAdfsMetadataEndpoint-輸入 HTTPs：//[您的 AD FS hostname]/federationmetadata/2007-06/federationmetadata.xml 
       - ida： OBOWebAPIBase-在上述 AD FS 區段中，輸入應用程式註冊 #19 的識別碼值。 
       - ida：授權單位-輸入 HTTPs：//[您的 AD FS hostname]/adfs 
  
          ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi26.png) 

 5. 開啟 WebAPIOBO 底下的 web.config 檔案。 修改下列各項： 
       - idaAdfsMetadataEndpoint-輸入 HTTPs：//[您的 AD FS hostname]/federationmetadata/2007-06/federationmetadata.xml 
       - ida：物件-在上 AD FS 區段中，從應用程式註冊 #12 輸入 [用戶端識別碼] 值 
 
          ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi27.png)
 
## <a name="test-the-sample"></a>測試範例 

本節說明如何測試上述設定的範例。 

程式碼變更之後，請重新建立解決方案 
 
  1. 在 Visual Studio 上，以滑鼠右鍵按一下 [方案]，然後選取 [**設定啟始專案**...]。 
      
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi28.png)

  2. 在 [屬性] 頁面上，請確定每個專案的 [**動作**] 都設為 [**啟動**]，但 TodoListSPA 除外。  
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi29.png)
  
  3. 在 Visual Studio 的頂端，按一下綠色箭號。  
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi30.png)

  4. 在原生應用程式的主畫面上，按一下 [登**入**]。 
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi31.png)

     如果您沒有看到 [原生應用程式] 畫面，請在系統上儲存專案存放庫的資料夾中，搜尋並移除 * msalcache. bin 檔案。 
  
  5. 系統會將您重新導向至 AD FS 登入頁面。 請繼續並登入。 
  
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi32.png)

  6. 登入之後，請在 [**建立待辦**事項] 專案中，輸入 Web api 呼叫的文字 web api。 按一下 [**新增專案**]。  這會呼叫 web API （待辦事項清單服務），然後呼叫 Web API 2 （WebAPIOBO），並在快取中新增專案。  
 
      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi33.png)
 
 ## <a name="next-steps"></a>後續步驟
[AD FS OpenID Connect/OAuth 流程和應用程式案例](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 
 
