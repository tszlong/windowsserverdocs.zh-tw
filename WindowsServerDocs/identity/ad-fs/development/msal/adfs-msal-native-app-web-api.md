---
title: AD FS MSAL 原生應用程式呼叫 Web API
description: 瞭解如何建立原生應用程式登入 2019 AD FS 使用者，並使用 MSAL 程式庫來取得權杖，以呼叫 web Api。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.openlocfilehash: 88333427cf8f147e0d81d8d07dfff08fb2cebb09
ms.sourcegitcommit: fc2a7c69a74edcd79372054c4a9a24237510babd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2021
ms.locfileid: "98672960"
---
# <a name="scenario-native-app-calling-web-api"></a>案例：呼叫 Web API 的原生應用程式
>適用于： AD FS 2019 和更新版本

瞭解如何建立原生應用程式登入 2019 AD FS 使用者，並使用 [MSAL 程式庫](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)  來取得權杖，以呼叫 web api。

閱讀本文之前，您應該先熟悉 [AD FS 概念](../ad-fs-openid-connect-oauth-concepts.md) 與 [授權碼授與流程](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)

## <a name="overview"></a>概觀

 ![概觀](media/adfs-msal-native-app-web-api/native1.png)

在此流程中，您會將驗證新增至您的原生應用程式 (公開用戶端) ，因此可讓使用者登入並呼叫 Web API。 若要從登入使用者的原生應用程式呼叫 Web API，您可以使用 MSAL 的 [AcquireTokenInteractive](/dotnet/api/microsoft.identity.client.ipublicclientapplication.acquiretokeninteractive#Microsoft_Identity_Client_IPublicClientApplication_AcquireTokenInteractive_System_Collections_Generic_IEnumerable_System_String__) token 取得方法。 為啟用此互動，MSAL 會利用網頁瀏覽器。

若要深入瞭解如何在 ADFS 中設定原生應用程式以互動方式取得存取權杖，讓我們使用 [這裡](https://github.com/microsoft/adfs-sample-msal-dotnet-native-to-webapi) 提供的範例，並逐步解說應用程式註冊和程式碼設定步驟。


## <a name="pre-requisites"></a>必要條件

- GitHub 用戶端工具
- AD FS 2019 或更新版本已設定並執行
- Visual Studio 2013 或更新版本

## <a name="app-registration-in-ad-fs"></a>AD FS 中的應用程式註冊
本節說明如何將原生應用程式註冊為公用用戶端和 Web API，做為信賴憑證者 (RP) 在 AD FS

  1. 在 **AD FS 管理**] 中，以滑鼠右鍵按一下 [ **應用程式群組** ]，然後選取 [ **新增應用程式群組**]。

  2. 在 [應用程式群組] 嚮導中，針對 [ **名稱** ] 輸入 **NativeAppToWebApi** ，在 [ **用戶端-伺服器應用程式** ] 下，選取 **存取 Web API 範本的原生應用程式** 。 按一下 [下一步] 。

      ![[新增應用程式群組] Wizard [歡迎使用] 頁面的螢幕擷取畫面，其中顯示已反白顯示存取 Web API 範本的原生應用程式。](media/adfs-msal-native-app-web-api/native2.png)

  3. 複製 **用戶端識別碼** 值。 稍後將用來作為應用程式 **App.config** 檔中的 **ClientId** 值。 針對 [重新導向 URI] 輸入下列 **內容：** https://ToDoListClient 。 按一下 **[新增]** 。 按一下 [下一步] 。

     ![[新增應用程式群組] Wizard 的 [原生應用程式] 頁面的螢幕擷取畫面，其中顯示重新導向 U R I。](media/adfs-msal-native-app-web-api/native3.png)

  4. 在 [設定 Web API] 畫面上，輸入 [ **識別碼：** ] https://localhost:44321/ 。 按一下 **[新增]** 。 按一下 [下一步] 。 此值稍後會在應用程式的 **App.config** 和 **Web.config** 檔案中使用。

     ![[新增應用程式群組] 頁面的 [設定 Web API] 頁面的螢幕擷取畫面，其中顯示正確的識別碼。](media/adfs-msal-native-app-web-api/native4.png)

  5. 在 [套用存取控制原則] 畫面上，選取 [**允許所有人**]，然後按 **[下一步**

     ![[新增應用程式群組] Wizard 的 [選擇存取控制原則] 頁面的螢幕擷取畫面，其中顯示已醒目提示 [允許每個人] 選項。](media/adfs-msal-native-app-web-api/native5.png)

  6. 在 [設定應用程式許可權] 畫面上，確認已選取 [ **Openid** ]，然後按 **[下一步]**。

     ![[新增應用程式群組] 的 [設定應用程式許可權] 頁面的螢幕擷取畫面，其中顯示已選取 [開啟我的]。](media/adfs-msal-native-app-web-api/native6.png)

  7. 在 [摘要] 畫面上，按 **[下一步]**。

  8. 在 [完成] 畫面上，按一下 [ **關閉**]。

  9. 在 AD FS 管理] 中，按一下 [ **應用程式群組** ]，然後選取 [ **NativeAppToWebApi**         應用程式群組]。 按一下滑鼠右鍵並選取 [內容]。

      ![[D F S 管理] 對話方塊的螢幕擷取畫面，其中顯示反白顯示的 NativeAppToWebApi 群組和下拉式清單中的 [屬性] 選項。](media/adfs-msal-native-app-web-api/native7.png)

  10. 在 [NativeAppToWebApi 屬性] 畫面上，選取 [ **WEB api** ] 下的 [ **NATIVEAPPTOWEBAPI – web api** ]，然後按一下 [**編輯**]。

      ![[NativeAppToWebApi 屬性] 對話方塊的螢幕擷取畫面，其中顯示已醒目提示 NativeAppToWebApi Web 的 P I 應用程式。](media/adfs-msal-native-app-web-api/native8.png)

  11. 在 [NativeAppToWebApi – Web API 屬性] 畫面上，選取 [**發行轉換規則**] 索引標籤，然後按一下 [**新增規則 ...** ]。

      ![顯示 [發行轉換規則] 索引標籤之 [NativeAppToWebApi-Web A P I 屬性] 對話方塊的螢幕擷取畫面。](media/adfs-msal-native-app-web-api/native9.png)

  12. 在 [新增轉換宣告規則] 中，選取 [從宣告 **規則範本****轉換連入** 宣告：] 下拉式清單，然後按 **[下一步]**。

      ![[新增轉換宣告規則] 頁面的 [選取規則範本] 頁面的螢幕擷取畫面，其中顯示已選取 [轉換傳入宣告] 選項。](media/adfs-msal-native-app-web-api/native10.png)

  13. 在 [宣告 **規則名稱：** ] 欄位中輸入 **NameID** 。 選取連 **入宣告類型** 的 **名稱**：、**外寄宣告類型** 的 **名稱識別碼**：和 **外寄名稱識別碼格式** 的 **一般名稱**：。 按一下 **[完成]**。

      ![[新增轉換宣告規則] 的 [設定規則] 頁面的螢幕擷取畫面，其中顯示上述設定。](media/adfs-msal-native-app-web-api/native11.png)

  14. 在 [NativeAppToWebApi – Web API 屬性] 畫面上按一下 [確定]，然後按一下 [NativeAppToWebApi 屬性] 畫面。

## <a name="code-configuration"></a>程式碼設定
本節說明如何將原生應用程式設定為登入使用者，並取得權杖以呼叫 Web API

1. 從[這裡](https://github.com/microsoft/adfs-sample-msal-dotnet-native-to-webapi)下載範例

2. 使用 Visual Studio 開啟範例

3. 開啟 App.config 檔。 修改下列各項：
   - ida：授權單位-輸入 h`ttps://[your AD FS hostname]/adfs`
   - ida： ClientId-在上述 AD FS 區段中，輸入應用程式註冊 #3 中的 **用戶端識別碼** 值。
   - ida： RedirectUri-在上述 AD FS 區段中，輸入應用程式註冊 #3 中的重新 **導向 URI** 值。
   - todo： TodoListResourceId –在上述 AD FS 區段中，輸入應用程式註冊 #4 中的 **識別碼** 值
   - ida： todo： TodoListBaseAddress-在上述 AD FS 區段中，輸入應用程式註冊 #4 中的 **識別碼** 值。

     ![應用程式佈建檔的螢幕擷取畫面，其中顯示已修改的值。](media/adfs-msal-native-app-web-api/native12.png)

 4. 開啟 Web.config 檔案。 修改下列各項：
    - ida：物件-在上述 AD FS 區段中，輸入應用程式註冊 #4 中的 **識別碼** 值
    - ida： AdfsMetadataEndpoint-enter `https://[your AD FS hostname]/federationmetadata/2007-06/federationmetadata.xml`

      ![Web 設定檔的螢幕擷取畫面，其中顯示修改過的值。](media/adfs-msal-native-app-web-api/native13.png)

## <a name="test-the-sample"></a>測試範例
本節說明如何測試上述設定的範例。

  1. 一旦變更程式碼之後，就會重新建立解決方案

  2. 在 Visual Studio 上，以滑鼠右鍵按一下 [方案]，然後選取 [**設定啟始專案**]。

     ![當您以滑鼠右鍵按一下已反白顯示 [設定啟動專案] 選項的方案時，所顯示清單的螢幕擷取畫面。](media/adfs-msal-native-app-web-api/native14.png)

  3. 在 [屬性] 頁面上，確定每個專案的 [ **動作** ] 設定為 [ **啟動** ]

     ![[方案屬性頁] 對話方塊的螢幕擷取畫面，其中顯示已選取 [多個啟始專案] 選項，且已將所有專案的動作設定為 [啟動]。](media/adfs-msal-native-app-web-api/native15.png)

  4. 在 Visual Studio 的頂端，按一下綠色箭號。

     ![已呼叫 [開始] 選項的 Visual Studio UI 螢幕擷取畫面。](media/adfs-msal-native-app-web-api/native16.png)

  5. 在原生應用程式的主畫面上，按一下 [登 **入**]。

     ![[待辦事項清單用戶端] 對話方塊的螢幕擷取畫面。](media/adfs-msal-native-app-web-api/native17.png)

   如果您沒有看到原生應用程式畫面，請在 `*msalcache.bin` 系統上儲存專案存放庫的資料夾中搜尋和移除檔案。

  1. 系統會將您重新導向至 AD FS 登入頁面。 繼續進行並登入。

      ![登入頁面的螢幕擷取畫面。](media/adfs-msal-native-app-web-api/native18.png)

  2. 登入後，請在 [**建立待辦事項**] 中輸入 Text **Build 原生應用程式到 Web Api** 。 按一下 [ **加入專案**]。  這會呼叫 **To Do List Service (WEB API)** 並在快取中新增專案。

       ![[待辦事項清單用戶端] 對話方塊的螢幕擷取畫面，其中包含 [待辦事項] 專案的 [待辦事項] 區段。](media/adfs-msal-native-app-web-api/native19.png)

## <a name="next-steps"></a>後續步驟
[AD FS OpenID Connect/OAuth 流程和應用程式案例](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
