---
title: 'AD FS MSAL Web API (代表案例來呼叫 Web API) '
description: 瞭解如何建立呼叫另一個 Web API 的 Web API。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.openlocfilehash: d5289caaa272931f2e96d6fafb410e9861896dfc
ms.sourcegitcommit: fc2a7c69a74edcd79372054c4a9a24237510babd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2021
ms.locfileid: "98672970"
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

      ![[新增應用程式群組] Wizard [歡迎使用] 頁面的螢幕擷取畫面，其中顯示已反白顯示存取 Web API 範本的原生應用程式。](media/adfs-msal-web-api-web-api/webapi2.png)

  3. 複製 **用戶端識別碼** 值。 稍後將用來作為應用程式 **App.config** 檔中的 **ClientId** 值。 針對 [重新導向 URI] 輸入下列 **內容：**  -  https://ToDoListClient 。 按一下 **[新增]** 。 按一下 [下一步] 。

      ![[新增應用程式群組] Wizard 的 [原生應用程式] 頁面的螢幕擷取畫面，其中顯示重新導向 U R I。](media/adfs-msal-web-api-web-api/webapi3.png)

  4. 在 [設定 Web API] 畫面上，輸入 [ **識別碼：** ] https://localhost:44321/ 。 按一下 **[新增]** 。 按一下 [下一步] 。 此值稍後會在應用程式的 **App.config** 和 **Web.Config** 檔案中使用。

      ![[新增應用程式群組] 頁面的 [設定 Web API] 頁面的螢幕擷取畫面，其中顯示正確的識別碼。](media/adfs-msal-web-api-web-api/webapi4.png)

  5. 在 [套用存取控制原則] 畫面上，選取 [**允許所有人**]，然後按 **[下一步**

      ![[新增應用程式群組] Wizard 的 [選擇存取控制原則] 頁面的螢幕擷取畫面，其中顯示已醒目提示 [允許每個人] 選項。](media/adfs-msal-web-api-web-api/webapi5.png)

  6. 在 [設定應用程式許可權] 畫面上，選取 [ **openid** ] 和 [ **user_impersonation**]。 按一下 [下一步] 。

      ![[新增應用程式群組] 的 [設定應用程式許可權] 頁面的螢幕擷取畫面，其中顯示已選取 [開啟我的]。](media/adfs-msal-web-api-web-api/webapi6.png)

  7. 在 [摘要] 畫面上，按 **[下一步]**。

  8. 在 [完成] 畫面上，按一下 [ **關閉**]。


  9. 在 AD FS 管理] 中，按一下 [ **應用程式群組** ]，然後選取 [ **WebApiToWebApi** 應用程式群組]。 按一下滑鼠右鍵並選取 [內容]。

      ![[D F S 管理] 對話方塊的螢幕擷取畫面，其中顯示反白顯示的 WebApiToWebApi 群組和下拉式清單中的 [屬性] 選項。](media/adfs-msal-web-api-web-api/webapi7.png)

  10. 在 [WebApiToWebApi 屬性] 畫面上，按一下 [ **新增應用程式 ...**]。

      ![[WebApiToWebApi 屬性] 對話方塊的螢幕擷取畫面，其中顯示列出的 WebApiToWebApi Web A P I 應用程式。](media/adfs-msal-web-api-web-api/webapi8.png)

  11. 在 [獨立應用程式] 下，選取 [ **伺服器應用程式**]。

      ![[加入新的應用程式至 WebApiToWebApi] wizard 的 [歡迎使用] 頁面的螢幕擷取畫面，其中顯示反白顯示的伺服器應用程式選項。](media/adfs-msal-web-api-web-api/webapi9.png)

  12. 在 [伺服器應用程式] 畫面上，加入 https://localhost:44321/ 做為 **用戶端識別碼** 和重新 **導向 URI**。

      ![[將新的應用程式新增至 WebApiToWebApi wizard] 頁面的 [伺服器應用程式] 頁面的螢幕擷取畫面，其中顯示正確的用戶端識別碼和重新導向 U R I。](media/adfs-msal-web-api-web-api/webapi10.png)

  13. 在 [設定應用程式認證] 畫面上，選取 [ **產生共用密碼**]。 複製密碼以供稍後使用。

      ![[將新的應用程式新增至 WebApiToWebApi wizard] 頁面的 [設定應用程式認證應用程式] 頁面的螢幕擷取畫面，其中顯示已選取 [產生共用密碼] 選項，並醒目提示產生的共用密碼。](media/adfs-msal-web-api-web-api/webapi11.png)

  14. 在 [摘要] 畫面上，按 **[下一步]**。

  15. 在 [完成] 畫面上，按一下 [ **關閉**]。

  16. 在 AD FS 管理] 中，按一下 [ **應用程式群組** ]，然後選取 [ **WebApiToWebApi** 應用程式群組]。 按一下滑鼠右鍵並選取 [內容]。

      ![[D F S 管理] 對話方塊的第二個螢幕擷取畫面，其中顯示反白顯示的 WebApiToWebApi 群組和下拉式清單中的 [屬性] 選項。](media/adfs-msal-web-api-web-api/webapi12.png)

  17. 在 [WebApiToWebApi 屬性] 畫面上，按一下 [ **新增應用程式 ...**]。

      ![[WebApiToWebApi 屬性] 對話方塊的第二個螢幕擷取畫面，其中顯示列出的 WebApiToWebApi Web A P I 應用程式。](media/adfs-msal-web-api-web-api/webapi13.png)

  18. 在 [獨立應用程式] 下，選取 [ **WEB API**]。

      ![[將新的應用程式新增至 WebApiToWebApi wizard] 頁面的 [歡迎使用] 頁面的螢幕擷取畫面，其中醒目提示 [Web A P I] 選項。](media/adfs-msal-web-api-web-api/webapi14.png)

  19. 在 [設定 Web API] 上，將新增 https://localhost:44300 為 **識別碼**。

      ![[將新的應用程式新增至 WebApiToWebApi wizard] 頁面的 [設定 Web A P I] 頁面的螢幕擷取畫面，其中顯示正確的「重新導向 U R I」。](media/adfs-msal-web-api-web-api/webapi15.png)

  20. 在 [套用存取控制原則] 畫面上，選取 [**允許所有人**]，然後按 **[下一步**

      ![[將新的應用程式新增至 WebApiToWebApi wizard] 頁面的 [選擇存取控制原則] 頁面的螢幕擷取畫面，其中顯示已醒目提示 [允許 everyone] 選項。](media/adfs-msal-web-api-web-api/webapi16.png)

  21. 在 [設定應用程式許可權] 畫面上，按 **[下一步]**。

      ![[將新的應用程式新增至 WebApiToWebApi wizard] 頁面的 [設定應用程式許可權] 頁面的螢幕擷取畫面，其中顯示已呼叫的下一個選項。](media/adfs-msal-web-api-web-api/webapi17.png)

  22. 在 [摘要] 畫面上，按 **[下一步]**。

  23. 在 [完成] 畫面上，按一下 [ **關閉**]。

  24. 在 WebApiToWebApi-Web API 2 屬性畫面上按一下 [確定]

  25. 在 [WebApiToWebApi 屬性] 畫面上，選取 [ **WebApiToWebApi – WEB API** ]，然後按一下 [ **編輯**]。

      ![[WebApiToWebApi 屬性] 對話方塊的螢幕擷取畫面，其中顯示已醒目提示 WebApiToWebApi Web 的 P I 應用程式。](media/adfs-msal-web-api-web-api/webapi18.png)

  26. 在 [WebApiToWebApi – Web API 屬性] 畫面上，選取 [ **發行轉換規則** ] 索引標籤，然後按一下 [ **新增規則 ...**]。

      ![顯示 [發行轉換規則] 索引標籤之 [WebApiToWebApi-Web A P I 屬性] 對話方塊的螢幕擷取畫面。](media/adfs-msal-web-api-web-api/webapi19.png)

  27. 在 [新增轉換宣告規則] 中，從下拉式清單選取 [ **使用自訂規則傳送宣告** ] 並按 **[下一步]**。

      ![[新增轉換宣告規則] 的 [選取規則範本] 頁面的螢幕擷取畫面，其中顯示已選取 [使用自訂規則傳送宣告] 選項。](media/adfs-msal-web-api-web-api/webapi20.png)

  28. 在 [宣告 **規則名稱：** 欄位和 **x： [] => (問題**] 中輸入 **PassAllClaims** [宣告 = x) ; 自訂規則：欄位中的宣告規則]，然後按一下 [完成]。

      ![[新增轉換宣告規則] 的 [設定規則] 頁面的螢幕擷取畫面，其中顯示上述設定。](media/adfs-msal-web-api-web-api/webapi21.png)

  29. 在 WebApiToWebApi-Web API 屬性畫面上按一下 [確定]

  30. 在 [WebApiToWebApi 屬性] 畫面上，選取 [選取 WebApiToWebApi – Web API 2]，然後按一下 [編輯]。</br>
  ![[WebApiToWebApi 屬性] 對話方塊的螢幕擷取畫面，其中顯示反白顯示的 WebApiToWebApi Web A P I 2 應用程式。](media/adfs-msal-web-api-web-api/webapi22.png)

  31. 在 [WebApiToWebApi – Web API 2 屬性] 畫面上，選取 [發行轉換規則] 索引標籤，然後按一下 [新增規則 ...]。

  32. 在 [新增轉換宣告規則] 嚮導中，從下拉式清單選取 [使用自訂規則傳送宣告]，然後按一下 ![ [新增轉換宣告規則] 頁面的 [選取規則範本] 頁面的 [下一步] 螢幕擷取畫面，其中顯示已選取 [使用自訂規則傳送宣告]](media/adfs-msal-web-api-web-api/webapi23.png)

  33. 在 [宣告規則名稱：欄位和 **x： [] => (問題] 中輸入 PassAllClaims [宣告 = x) ;** **自訂規則：** 欄位中的宣告規則]，然後按一下 **[完成]**。

      ![[新增轉換宣告規則] 頁面的 [設定規則] 頁面的第二個螢幕擷取畫面，其中顯示上述設定。](media/adfs-msal-web-api-web-api/webapi24.png)

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

            ![應用程式佈建檔的螢幕擷取畫面，其中顯示已修改的值。](media/adfs-msal-web-api-web-api/webapi25.png)

  4. 開啟 ToDoListService 下的 Web.config 檔案。 修改下列各項：
       - ida：物件-在上述 AD FS 區段中，輸入應用程式註冊 #12 中的用戶端識別碼值
       - ida： ClientId-在上述 AD FS 區段中，輸入應用程式註冊 #12 中的用戶端識別碼值。
       - Ida： ClientSecret-輸入在上述 AD FS 區段中，從應用程式註冊 #13 複製的共用秘密。
       - ida： RedirectUri-在上述 AD FS 區段中，輸入應用程式註冊 #12 中的 RedirectUri 值。
       - ida： AdfsMetadataEndpoint-輸入 HTTPs：//[您的 AD FS hostname]/federationmetadata/2007-06/federationmetadata.xml
       - ida： OBOWebAPIBase-在上述 AD FS 區段中，輸入應用程式註冊 #19 中的識別碼值。
       - ida：授權單位輸入 HTTPs：//[您的 AD FS 主機名稱]/adfs

          ![ToDoListService 下 web 設定檔的螢幕擷取畫面，其中顯示修改過的值。](media/adfs-msal-web-api-web-api/webapi26.png)

 5. 開啟 WebAPIOBO 下的 Web.config 檔案。 修改下列各項：
       - ida： AdfsMetadataEndpoint-輸入 HTTPs：//[您的 AD FS hostname]/federationmetadata/2007-06/federationmetadata.xml
       - ida：物件-在上述 AD FS 區段中，輸入應用程式註冊 #12 中的用戶端識別碼值

          ![WebAPIOBO 下 web 設定檔的螢幕擷取畫面，其中顯示修改過的值。](media/adfs-msal-web-api-web-api/webapi27.png)

## <a name="test-the-sample"></a>測試範例

本節說明如何測試上述設定的範例。

一旦變更程式碼之後，就會重新建立解決方案

  1. 在 Visual Studio 上，以滑鼠右鍵按一下 [方案]，然後選取 [**設定啟始專案**]。

      ![當您以滑鼠右鍵按一下已反白顯示 [設定啟動專案] 選項的方案時，所顯示清單的螢幕擷取畫面。](media/adfs-msal-web-api-web-api/webapi28.png)

  2. 在 [屬性] 頁面上，請確定每個專案的 [ **動作** ] 設定為 [ **啟動** ]，但 TodoListSPA 除外。

      ![[方案屬性頁] 對話方塊的螢幕擷取畫面，其中顯示已選取 [多個啟始專案] 選項，且已將所有專案的動作設定為 [啟動]。](media/adfs-msal-web-api-web-api/webapi29.png)

  3. 在 Visual Studio 的頂端，按一下綠色箭號。

      ![已呼叫 [開始] 選項的 Visual Studio UI 螢幕擷取畫面。](media/adfs-msal-web-api-web-api/webapi30.png)

  4. 在原生應用程式的主畫面上，按一下 [登 **入**]。

      ![[待辦事項清單用戶端] 對話方塊的螢幕擷取畫面。](media/adfs-msal-web-api-web-api/webapi31.png)

     如果您沒有看到原生應用程式畫面，請在系統上儲存專案存放庫的資料夾中搜尋和移除 * msalcache。

  5. 系統會將您重新導向至 AD FS 登入頁面。 繼續進行並登入。

      ![登入頁面的螢幕擷取畫面。](media/adfs-msal-web-api-web-api/webapi32.png)

  6. 登入後，請在 [ **建立待辦事項**] 中輸入文字 web Api 對 web Api 的呼叫。 按一下 [ **加入專案**]。  這會呼叫 Web API (待辦事項清單服務) 該服務接著會呼叫 Web API 2 (WebAPIOBO) 並在快取中新增專案。

      ![[待辦事項清單用戶端] 對話方塊的螢幕擷取畫面，其中包含 [待辦事項] 專案的 [待辦事項] 區段。](media/adfs-msal-web-api-web-api/webapi33.png)

 ## <a name="next-steps"></a>後續步驟
[AD FS OpenID Connect/OAuth 流程和應用程式案例](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)


