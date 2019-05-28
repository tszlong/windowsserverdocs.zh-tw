---
title: 建置原生用戶端應用程式使用 OAuth 公用用戶端與 AD FS 2016 或更新版本
description: 提供建置原生用戶端應用程式使用 OAuth 公用用戶端和 AD FS 2016 或更新版本的指示逐步解說
author: billmath
ms.author: billmath
ms.reviewer: anandy
manager: mtillman
ms.date: 07/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: e85a97fa08e4c77588b17aee08ee03e0b897a74c
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976862"
---
# <a name="build-a-native-client-application-using-oauth-public-clients-with-ad-fs-2016-or-later"></a>建置原生用戶端應用程式使用 OAuth 公用用戶端與 AD FS 2016 或更新版本

## <a name="overview"></a>總覽

這篇文章會示範如何建置 Web API 受到 AD FS 2016 或更新版本進行互動的原生應用程式。

1. .Net TodoListClient WPF 應用程式會從 Azure Active Directory (Azure AD) 取得 JWT 存取權杖，透過 OAuth 2.0 通訊協定來使用 Active Directory Authentication Library (ADAL)
2. 存取權杖做為持有人權杖來驗證使用者時呼叫 /todolist 端點 TodoListService web API。
 我們將使用以下的 Azure ad 的應用程式範例，然後將它修改適用於 AD FS 2016 或更新版本。

![應用程式概觀](media/native-client-with-ad-fs-2016/appoverview.png)

## <a name="pre-requisites"></a>必要條件
以下是所需完成這份文件之前的必要元件的清單。 本文件假設已安裝 AD FS，並已建立的 AD FS 伺服器陣列。

* GitHub 用戶端工具
* 在 Windows Server 2016 或更新版本的 AD FS
* Visual Studio 2013 或更新版本

## <a name="creating-the-sample-walkthrough"></a>建立範例逐步解說

### <a name="create-the-application-group-in-ad-fs"></a>在 AD FS 中建立應用程式群組

1. 在 AD FS 管理中，以滑鼠右鍵按一下**應用程式群組**，然後選取**新增應用程式群組**。

2. 在 [應用程式群組精靈] 中，做為名稱輸入任何名稱想，例如 NativeToDoListAppGroup。 選取 **存取 web API 的原生應用程式**範本。 按一下 [下一步]  。
 ![新增應用程式群組](media/native-client-with-ad-fs-2016/addapplicationgroup1.png)

3. 在 **原生應用程式**頁面上，記下 AD FS 所產生的識別碼。 這是與 AD FS 會辨識公用用戶端應用程式的識別碼。 複製**用戶端識別元**值。 它將在稍後用做為值**ida: ClientId**應用程式程式碼中。 如果您想要您可以讓任何自訂的識別項。 重新導向 URI 是任何任意的值，例如放 https://ToDoListClient![原生應用程式](media/native-client-with-ad-fs-2016/addapplicationgroup2.png)

4. 在 **設定 Web API**頁面上，將 Web api 的識別碼值。 針對此範例中，這應該是 windows 7 **SSL URL** Web 應用程式應在其中執行。 您可以按一下 TooListServer 專案方案中的屬性，以取得此值。 這將會稍後再做**todo:TodoListResourceId**中的值**App.config**檔案的原生用戶端應用程式，並可作為**todo:TodoListBaseAddress**。
![Web API](media/native-client-with-ad-fs-2016/addapplicationgroup3.png)

5. 逐步**套用存取控制原則**並**設定的應用程式權限**就地的預設值。 [摘要] 頁面應該看起來如下。
![摘要](media/native-client-with-ad-fs-2016/addapplicationgroupsummary.png)

按一下 下一步，然後完成精靈。

### <a name="add-the-nameidentifier-claim-to-the-list-of-claims-issued"></a>將 NameIdentifier 宣告必須發出的宣告清單
示範應用程式的不同位置的 NameIdentifier 宣告中使用的值。 不同於 Azure AD 中，AD FS 不會預設發出一個 NameIdentifier 宣告。 因此，我們需要新增宣告規則以發出 NameIdentifier 宣告，好讓應用程式可以使用正確的值。 在此範例中，使用者的名字是以 NameIdentifier 值的使用者權杖中發出。
若要設定宣告規則，請開啟剛才建立的應用程式群組，並按兩下 Web API。 選取 [發佈轉換規則] 索引標籤，然後按一下 [新增規則] 按鈕。 在宣告規則的類型，選擇 自訂宣告規則，然後新增宣告規則，如下所示。

```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
 => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier"), query = ";givenName;{0}", param = c.Value);
```

![NameIdentifier 宣告規則](media/native-client-with-ad-fs-2016/addnameidentifierclaimrule.png)

### <a name="modify-the-application-code"></a>修改應用程式程式碼

本節討論如何下載之範例 Web API，並在 Visual Studio 中修改它。   我們將使用 Azure AD 範例所[此處](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop)。  

若要下載範例專案，使用 Git Bash，並輸入下列命令：  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-native-desktop  
```  

#### <a name="modify-todolistclient"></a>修改 ToDoListClient

此方案中的專案表示原生用戶端應用程式。 我們需要確定用戶端應用程式知道：

1. 要如何取得驗證時所需的使用者在何處？
2. 什麼是識別碼該用戶端必須提供驗證授權單位 (AD FS)？
3. 我們正要求的存取權杖的資源識別碼為何？
4. 什麼是 Web API 的基底位址？

若要取得上述資訊給原生用戶端應用程式需要下列的程式碼變更。

**App.config**

* 新增金鑰**ida： 授權單位**描述 AD FS 服務的值。 例如： https://fs.contoso.com/adfs/ 
* 修改**ida: ClientId**的值索引鍵**用戶端識別碼**中**原生應用程式**應用程式群組建立期間在 AD FS 中的頁面。 比方說，3f07368b-6efd-4f50-a330-d93853f4c855
* 修改**todo:todo:TodoListResourceId**中的值取代**識別項**中**設定 Web API**應用程式群組建立期間在 AD FS 中的頁面。 例如： https://localhost:44321/ 
* 修改**todo:TodoListBaseAddress**中的值取代**識別項**中**設定 Web API**應用程式群組建立期間在 AD FS 中的頁面。 例如： https://localhost:44321/ 
* 設定的值**ida: RedirectUri**中的值取代**重新導向 URI**中**原生應用程式**應用程式群組建立期間在 AD FS 中的頁面。 例如： https://ToDoListClient 
* 為了方便閱讀，您可以移除 / 註解的索引鍵**ida： 租用戶**並**ida: AADInstance**。

  ![應用程式設定](media/native-client-with-ad-fs-2016/app_configfile.PNG)


**MainWindow.xaml.cs**

* 註解的線條，如下所示的 aadInstance

        // private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];

* 新增授權單位的值，如下所示

        private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

* 刪除建立的那一行**授權單位**aadInstance 和租用戶中的值

        private static string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);

* 在函式**MainWindow**，變更至 Aquiretoken 具現化

        authContext = new AuthenticationContext(authority,false);

    ADAL 不支援驗證 AD FS 做為授權單位，因此我們必須傳遞 false 值的旗標 validateAuthority 參數。

  ![主視窗](media/native-client-with-ad-fs-2016/mainwindow.PNG)

#### <a name="modify-todolistservice"></a>修改 TodoListService
您必須變更此專案 – Web.config 和 Startup.Auth.cs 兩個檔案。 Web.Config 變更，才能取得正確的參數值。 若要設定 AD FS，而不是 Azure AD 進行驗證，WebAPI 需要 Startup.Auth.cs 變更。

**Web.config**

* 註解的索引鍵**ida： 租用戶**因為我們不需要它
* 新增的索引鍵**ida： 授權單位**值，表示同盟的 FQDN 與服務，例如 https://fs.contoso.com/adfs/
* 修改索引鍵**ida： 對象**的值是您在中指定的 Web API 識別碼**設定 Web API**期間在 AD FS 中的 新增應用程式群組的頁面。
* 新增金鑰**ida: AdfsMetadataEndpoint**值對應到 AD FS 的同盟中繼資料 URL 與服務，例如： https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

![Web 設定](media/native-client-with-ad-fs-2016/webconfig.PNG)


**Startup.Auth.cs**

修改 ConfigureAuth 函式，如下所示

    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseActiveDirectoryFederationServicesBearerAuthentication(
            new ActiveDirectoryFederationServicesBearerAuthenticationOptions
            {
                MetadataEndpoint = ConfigurationManager.AppSettings["ida:AdfsMetadataEndpoint"],
                TokenValidationParameters = new TokenValidationParameters()
                {
                    SaveSigninToken = true,
                    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"]
                }

            });
    }

基本上，我們會設定使用 AD FS，並進一步提供 AD FS 中繼資料的相關資訊，以及驗證權杖的驗證，audience 宣告應該是 Web API 所預期的值。
執行應用程式

1. Nativeclient-dotnet 方案，以滑鼠右鍵按一下並移至屬性。 將如下所示的啟始專案變更為多個啟始專案，並設定 TodoListClient 和 TodoListService 都為啟動。
![方案屬性](media/native-client-with-ad-fs-2016/solutionproperties.png)

2.  請按 F5 按鈕或選取 偵錯 > 繼續在功能表列中的。 這會啟動原生應用程式和 WebAPI。 按一下 登入按鈕上的原生應用程式，則會快顯視窗中從 AD AL 的互動式登入，並將重新導向至您的 AD FS 服務。 輸入有效的使用者認證。
![登入](media/native-client-with-ad-fs-2016/sign-in.png)

在此步驟中，原生應用程式重新導向至 AD FS 和 Web api 收到 ID 權杖和存取權杖

3.  輸入執行 文字 方塊中的項目，然後按一下 新增項目。 在此步驟中，應用程式向外連到 Web API，以新增待辦事項項目，以及若要這樣做，請提供存取權杖來取得從 AD FS 的 WebAPI。 Web API 會比對的對象數值，以確認權杖是針對它，因此會驗證權杖的簽章使用從同盟中繼資料資訊。

![登入](media/native-client-with-ad-fs-2016/clienttodoadd.png)

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
