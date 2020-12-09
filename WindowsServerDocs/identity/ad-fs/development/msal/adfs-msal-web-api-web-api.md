---
title: 'AD FS MSAL Web API (代表案例來呼叫 Web API) '
description: 瞭解如何建立呼叫另一個 Web API 的 Web API。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.openlocfilehash: a8d3bb8c7094316dc8b54430137cac0c4fa7c75e
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866417"
---
# <a name="scenario-web-api-calling-web-api-on-behalf-of-scenario"></a>案例： (代表案例呼叫 Web API 的 Web API) 
> 適用于： AD FS 2019 和更新版本

瞭解如何建立 Web API，以代表使用者呼叫另一個 Web API。

閱讀本文之前，您應該先熟悉 [AD FS 概念](../ad-fs-openid-connect-oauth-concepts.md) 和 [內部 Behalf_Of 流程](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#on-behalf-of-flow)

## <a name="overview"></a>概觀


- 用戶端 (Web 應用程式) -不會在下圖中表示-呼叫受保護的 Web API，並在其「授權」 Http 標頭中提供 JWT 持有人權杖。
- 受保護的 Web API 會驗證權杖，並使用 MSAL [AcquireTokenOnBehalfOf](/dotnet/api/microsoft.identitymodel.clients.activedirectory.authenticationcontext.acquiretokenasync#Microsoft_IdentityModel_Clients_ActiveDirectory_AuthenticationContext_AcquireTokenAsync_System_String_Microsoft_IdentityModel_Clients_ActiveDirectory_ClientCredential_Microsoft_IdentityModel_Clients_ActiveDirectory_UserAssertion_)   方法，從 AD FS) 另一個權杖要求 (，讓它本身可以代表使用者呼叫名為下游 Web) api 的第二個 web api (。
- 受保護的 web API 會使用此權杖來呼叫下游 API。 它也可以呼叫 AcquireTokenSilentlater 來要求其他下游 Api 的權杖 (但仍代表相同的使用者) 。AcquireTokenSilent 會視需要重新整理權杖。

     ![概觀](media/adfs-msal-web-api-web-api/webapi1.png)

若要進一步瞭解如何在 ADFS 中代表驗證案例進行設定，讓我們使用 [這裡](https://github.com/microsoft/adfs-sample-msal-dotnet-webapi-to-webapi-onbehalfof) 提供的範例，並逐步解說應用程式註冊和程式碼設定步驟。

## <a name="pre-requisites"></a>必要條件

- GitHub 用戶端工具
- AD FS 2019 或更新版本已設定並執行
- Visual Studio 2013 或更新版本

## <a name="app-registration-in-ad-fs"></a>AD FS 中的應用程式註冊

本節說明如何將原生應用程式註冊為公用用戶端和 Web Api，做為信賴憑證者在 AD FS 中 (RP) 

  1. 在 AD FS 管理] 中，以滑鼠右鍵按一下 [ **應用程式群組** ]，然後選取 [ **新增應用程式群組**]。

  2. 在 [應用程式群組] 嚮導中，針對 [ **名稱** ] 輸入 **WebApiToWebApi** ，在 [ **用戶端-伺服器應用程式** ] 下，選取 **存取 Web API 範本的原生應用程式** 。 按一下 [下一步] 。

      ![App 註冊](media/adfs-msal-web-api-web-api/webapi2.png)

  3. 複製 **用戶端識別碼** 值。 稍後將用來作為應用程式 **App.config** 檔中的 **ClientId** 值。 針對 [重新導向 URI] 輸入下列 **內容：**  -  https://ToDoListClient 。 按一下 [新增]  。 按一下 [下一步] 。

      ![App 註冊](media/adfs-msal-web-api-web-api/webapi3.png)

  4. 在 [設定 Web API] 畫面上，輸入 [ **識別碼：** ] https://localhost:44321/ 。 按一下 [新增]  。 按一下 [下一步] 。 此值稍後會在應用程式的 **App.config** 和 **Web.Config** 檔案中使用。

      ![App 註冊](media/adfs-msal-web-api-web-api/webapi4.png)

  5. 在 [套用存取控制原則] 畫面上，選取 [**允許所有人**]，然後按 **[下一步**

      ![App 註冊](media/adfs-msal-web-api-web-api/webapi5.png)

  6. 在 [設定應用程式許可權] 畫面上，選取 [ **openid** ] 和 [ **user_impersonation**]。 按一下 [下一步] 。

      ![App 註冊](media/adfs-msal-web-api-web-api/webapi6.png)

  7. 在 [摘要] 畫面上，按 **[下一步]**。

  8. 在 [完成] 畫面上，按一下 [ **關閉**]。


  9. 在 AD FS 管理] 中，按一下 [ **應用程式群組** ]，然後選取 [ **WebApiToWebApi** 應用程式群組]。 按一下滑鼠右鍵並選取 [內容]。

      ![App 註冊](media/adfs-msal-web-api-web-api/webapi7.png)

  10. 在 [WebApiToWebApi 屬性] 畫面上，按一下 [ **新增應用程式 ...**]。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi8.png)

  11. 在 [獨立應用程式] 下，選取 [ **伺服器應用程式**]。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi9.png)

  12. 在 [伺服器應用程式] 畫面上，加入 https://localhost:44321/ 做為 **用戶端識別碼** 和重新 **導向 URI**。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi10.png)

  13. 在 [設定應用程式認證] 畫面上，選取 [ **產生共用密碼**]。 複製密碼以供稍後使用。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi11.png)

  14. 在 [摘要] 畫面上，按 **[下一步]**。

  15. 在 [完成] 畫面上，按一下 [ **關閉**]。

  16. 在 AD FS 管理] 中，按一下 [ **應用程式群組** ]，然後選取 [ **WebApiToWebApi** 應用程式群組]。 按一下滑鼠右鍵並選取 [內容]。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi12.png)

  17. 在 [WebApiToWebApi 屬性] 畫面上，按一下 [ **新增應用程式 ...**]。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi13.png)

  18. 在 [獨立應用程式] 下，選取 [ **WEB API**]。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi14.png)

  19. 在 [設定 Web API] 上，將新增 https://localhost:44300 為 **識別碼**。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi15.png)

  20. 在 [套用存取控制原則] 畫面上，選取 [**允許所有人**]，然後按 **[下一步**

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi16.png)

  21. 在 [設定應用程式許可權] 畫面上，按 **[下一步]**。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi17.png)

  22. 在 [摘要] 畫面上，按 **[下一步]**。

  23. 在 [完成] 畫面上，按一下 [ **關閉**]。

  24. 在 WebApiToWebApi-Web API 2 屬性畫面上按一下 [確定]

  25. 在 [WebApiToWebApi 屬性] 畫面上，選取 [ **WebApiToWebApi – WEB API** ]，然後按一下 [ **編輯**]。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi18.png)

  26. 在 [WebApiToWebApi – Web API 屬性] 畫面上，選取 [ **發行轉換規則** ] 索引標籤，然後按一下 [ **新增規則 ...**]。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi19.png)

  27. 在 [新增轉換宣告規則] 中，從下拉式清單選取 [ **使用自訂規則傳送宣告** ] 並按 **[下一步]**。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi20.png)

  28. 在 [宣告 **規則名稱：** 欄位和 **x： [] => (問題**] 中輸入 **PassAllClaims** [宣告 = x) ; 自訂規則：欄位中的宣告規則]，然後按一下 [完成]。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi21.png)

  29. 在 WebApiToWebApi-Web API 屬性畫面上按一下 [確定]

  30. 在 [WebApiToWebApi 屬性] 畫面上，選取 [選取 WebApiToWebApi – Web API 2]，然後按一下 [編輯]。</br>
  ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi22.png)

  31. 在 [WebApiToWebApi – Web API 2 屬性] 畫面上，選取 [發行轉換規則] 索引標籤，然後按一下 [新增規則 ...]。

  32. 在 [新增轉換宣告規則] 中，選取 [從 dopdown 使用自訂規則傳送宣告]，然後按一下 [下一次 ![ 應用程式註冊]](media/adfs-msal-web-api-web-api/webapi23.png)

  33. 在 [宣告規則名稱：欄位和 **x： [] => (問題] 中輸入 PassAllClaims [宣告 = x) ;** **自訂規則：** 欄位中的宣告規則]，然後按一下 **[完成]**。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi24.png)

  34.  在 [WebApiToWebApi – Web API 2 屬性] 畫面上按一下 [確定]，然後在 [WebApiToWebApi 屬性] 畫面上按一下。


## <a name="code-configuration"></a>程式碼設定

本節說明如何設定 Web API 以呼叫另一個 Web API

  1. 從[這裡](https://github.com/microsoft/adfs-sample-msal-dotnet-webapi-to-webapi-onbehalfof)下載範例

  2. 使用 Visual Studio 開啟範例

  3. 開啟 App.config 檔。 修改下列各項：
       - ida：授權單位輸入 HTTPs：//[您的 AD FS 主機名稱]/adfs/
       - ida： ClientId-在上述 AD FS 區段中輸入應用程式註冊 #3 的值。
       - ida： RedirectUri-在上述 AD FS 區段中輸入應用程式註冊 #3 的值。
       - todo： TodoListResourceId –在上述 AD FS 區段中，輸入應用程式註冊 #4 中的識別碼值
       - ida： todo： TodoListBaseAddress-在上述 AD FS 區段中，輸入應用程式註冊 #4 中的識別碼值。

            ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi25.png)

  4. 開啟 ToDoListService 下的 Web.config 檔案。 修改下列各項：
       - ida：物件-在上述 AD FS 區段中，輸入應用程式註冊 #12 中的用戶端識別碼值
       - ida： ClientId-在上述 AD FS 區段中，輸入應用程式註冊 #12 中的用戶端識別碼值。
       - Ida： ClientSecret-輸入在上述 AD FS 區段中，從應用程式註冊 #13 複製的共用秘密。
       - ida： RedirectUri-在上述 AD FS 區段中，輸入應用程式註冊 #12 中的 RedirectUri 值。
       - ida： AdfsMetadataEndpoint-輸入 HTTPs：//[您的 AD FS hostname]/federationmetadata/2007-06/federationmetadata.xml
       - ida： OBOWebAPIBase-在上述 AD FS 區段中，輸入應用程式註冊 #19 中的識別碼值。
       - ida：授權單位輸入 HTTPs：//[您的 AD FS 主機名稱]/adfs

          ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi26.png)

 5. 開啟 WebAPIOBO 下的 Web.config 檔案。 修改下列各項：
       - ida： AdfsMetadataEndpoint-輸入 HTTPs：//[您的 AD FS hostname]/federationmetadata/2007-06/federationmetadata.xml
       - ida：物件-在上述 AD FS 區段中，輸入應用程式註冊 #12 中的用戶端識別碼值

          ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi27.png)

## <a name="test-the-sample"></a>測試範例

本節說明如何測試上述設定的範例。

一旦變更程式碼之後，就會重新建立解決方案

  1. 在 Visual Studio 上，以滑鼠右鍵按一下 [方案]，然後選取 [**設定啟始專案**]。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi28.png)

  2. 在 [屬性] 頁面上，請確定每個專案的 [ **動作** ] 設定為 [ **啟動** ]，但 TodoListSPA 除外。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi29.png)

  3. 在 Visual Studio 的頂端，按一下綠色箭號。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi30.png)

  4. 在原生應用程式的主畫面上，按一下 [登 **入**]。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi31.png)

     如果您沒有看到原生應用程式畫面，請在系統上儲存專案存放庫的資料夾中搜尋和移除 * msalcache。

  5. 系統會將您重新導向至 AD FS 登入頁面。 繼續進行並登入。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi32.png)

  6. 登入後，請在 [ **建立待辦事項**] 中輸入文字 web Api 對 web Api 的呼叫。 按一下 [ **加入專案**]。  這會呼叫 Web API (待辦事項清單服務) 該服務接著會呼叫 Web API 2 (WebAPIOBO) 並在快取中新增專案。

      ![應用程式 Reg](media/adfs-msal-web-api-web-api/webapi33.png)

 ## <a name="next-steps"></a>後續步驟
[AD FS OpenID Connect/OAuth 流程和應用程式案例](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)


