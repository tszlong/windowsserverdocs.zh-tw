---
title: 使用 AD FS 2016 或更新版本的 OAuth 公用用戶端建立原生用戶端應用程式
description: 此逐步解說提供使用 OAuth 公用用戶端和 AD FS 2016 或更新版本來建立原生用戶端應用程式的指示
author: billmath
ms.author: billmath
ms.reviewer: anandy
manager: mtillman
ms.date: 07/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 2eb2d0a3cfa6763d308bbd73f1ccf50795b06e77
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866343"
---
# <a name="build-a-native-client-application-using-oauth-public-clients-with-ad-fs-2016-or-later"></a>使用 AD FS 2016 或更新版本的 OAuth 公用用戶端建立原生用戶端應用程式

## <a name="overview"></a>總覽

本文說明如何建立原生應用程式，以與受 AD FS 2016 或更新版本保護的 Web API 互動。

1. .Net TodoListClient WPF 應用程式會使用 Active Directory 驗證程式庫（ADAL），透過 OAuth 2.0 通訊協定從 Azure Active Directory （Azure AD）取得 JWT 存取權杖
2. 存取權杖是用來做為持有人權杖，以便在呼叫 TodoListService Web API 的/todolist 端點時驗證使用者。
 我們將在這裡使用 Azure AD 的應用程式範例，然後針對 AD FS 2016 或更新版本加以修改。

![應用程式概觀](media/native-client-with-ad-fs-2016/appoverview.png)

## <a name="pre-requisites"></a>先決條件
以下是完成本檔之前所需的先決條件清單。 本檔假設已安裝 AD FS，且已建立 AD FS 伺服器陣列。

* GitHub 用戶端工具
* Windows Server 2016 或更新版本中的 AD FS
* Visual Studio 2013 或更新版本

## <a name="creating-the-sample-walkthrough"></a>建立範例逐步解說

### <a name="create-the-application-group-in-ad-fs"></a>在 AD FS 中建立應用程式群組

1. 在 AD FS 管理 中，以滑鼠右鍵按一下 **應用程式群組**，然後選取 **新增應用程式群組**。

2. 在 [應用程式群組嚮導] 上，針對 [名稱] 輸入您偏好的任何名稱，例如 NativeToDoListAppGroup。 選取**存取 Web API 範本的原生應用程式**。 按一下 [下一步]。
 ![新增應用程式群組](media/native-client-with-ad-fs-2016/addapplicationgroup1.png)

3. 在 [**原生應用程式**] 頁面上，記下 AD FS 所產生的識別碼。 這是 AD FS 將用來辨識公用用戶端應用程式的識別碼。 複製 [**用戶端識別碼**] 值。 稍後在應用程式代碼中，將會使用它做為**ida： ClientId**的值。 如果您想要的話，可以在這裡提供任何自訂識別碼。 重新導向 URI 是任意值，例如 put https://ToDoListClient ![ 原生應用程式](media/native-client-with-ad-fs-2016/addapplicationgroup2.png)

4. 在 [設定**WEB api** ] 頁面上，設定 web api 的 [識別碼] 值。 在此範例中，這應該是 Web 應用程式應該執行的**SSL URL**值。 您可以按一下方案中 TooListServer 專案的屬性來取得此值。 稍後在 native client 應用程式**的 app.config 檔案中，** 這會做為**todo： TodoListResourceId**值，也會做為**todo： TodoListBaseAddress**。
![Web API](media/native-client-with-ad-fs-2016/addapplicationgroup3.png)

5. 流覽套用**存取控制原則**，並設定具有預設值的**應用程式許可權**。 [摘要] 頁面看起來應該如下所示。
![摘要](media/native-client-with-ad-fs-2016/addapplicationgroupsummary.png)

按 [下一步]，然後完成嚮導。

### <a name="add-the-nameidentifier-claim-to-the-list-of-claims-issued"></a>將 NameIdentifier 宣告新增至發出的宣告清單
示範應用程式會在不同位置使用 NameIdentifier 宣告中的值。 不同于 Azure AD，AD FS 預設不會發出 NameIdentifier 宣告。 因此，我們需要新增宣告規則來發出 NameIdentifier 宣告，讓應用程式可以使用正確的值。 在此範例中，會將使用者的指定名稱當做權杖中使用者的 NameIdentifier 值發出。
若要設定宣告規則，請開啟剛建立的應用程式群組，然後按兩下 Web API。 選取 [發行轉換規則] 索引標籤，然後按一下 [新增規則] 按鈕。 在宣告規則的類型中，選擇 [自訂宣告規則]，然後新增宣告規則，如下所示。

```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
 => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier"), query = ";givenName;{0}", param = c.Value);
```

![NameIdentifier 宣告規則](media/native-client-with-ad-fs-2016/addnameidentifierclaimrule.png)

### <a name="modify-the-application-code"></a>修改應用程式代碼

本節討論如何下載範例 Web API，並在 Visual Studio 中加以修改。   我們將使用[這裡](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop)的 Azure AD 範例。  

若要下載範例專案，請使用 Git Bash 並輸入下列內容：  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-native-desktop  
```  

#### <a name="modify-todolistclient"></a>修改 ToDoListClient

方案中的這個專案代表 native client 應用程式。 我們必須確保用戶端應用程式知道：

1. 何時可以在需要時，讓使用者進行驗證？
2. 用戶端需要提供多少識別碼給驗證授權單位（AD FS）？
3. 我們要求存取權杖的資源識別碼為何？
4. Web API 的基底位址為何？

需要進行下列程式碼變更，才能將上述資訊取得至原生用戶端應用程式。

**App.config**

* 使用描述 AD FS 服務的值，新增金鑰**ida：授權**單位。 例如： https://fs.contoso.com/adfs/
* 在 AD FS 中建立應用程式群組期間，使用**原生應用程式**頁面中的 [**用戶端識別碼**] 值來修改**ida： ClientId**金鑰。 例如，3f07368b-6efd-4f50-a330-d93853f4c855
* 在 AD FS 中建立應用程式群組期間，在 [**設定 WEB API** ] 頁面中，從 [**識別碼**] 的值修改**todo： todo： TodoListResourceId** 。 例如： https://localhost:44321/
* 在 AD FS 中建立應用程式群組期間，從 [**設定 WEB API** ] 頁面中的 [**識別碼**] 的值修改**todo： TodoListBaseAddress** 。 例如： https://localhost:44321/
* 在 AD FS 中建立應用程式群組期間，使用 [**原生應用程式**] 頁面中的 [重新**導向 URI** ] 的值來設定**ida： RedirectUri**的值。 例如： https://ToDoListClient
* 為了方便閱讀，您可以移除/批註**ida： Tenant**和**ida： AADInstance**的金鑰。

  ![應用程式設定](media/native-client-with-ad-fs-2016/app_configfile.PNG)


**MainWindow.xaml.cs**

* 批註 aadInstance 的程式程式碼，如下所示

        // private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];

* 新增 [授權] 的值，如下所示

        private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

* 刪除從 aadInstance 和租使用者建立**授權**單位值的程式程式碼

        private static string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);

* 在函式**mainwindow.xaml**中，將 authCoNtext 具現化變更為

        authContext = new AuthenticationContext(authority,false);

    ADAL 不支援以授權單位來驗證 AD FS，因此我們必須針對 validateAuthority 參數傳遞 false 值旗標。

  ![主視窗](media/native-client-with-ad-fs-2016/mainwindow.PNG)

#### <a name="modify-todolistservice"></a>修改 TodoListService
兩個檔案需要在此專案中變更– web.config 和 Startup.Auth.cs。 需要進行 web.config 變更，才能取得正確的參數值。 需要進行 Startup.Auth.cs 變更，才能將 WebAPI 設定為針對 AD FS 進行驗證，而不是 Azure AD。

**Web.config**

* 批註金鑰**ida：租**使用者，因為我們不需要它
* 新增**ida：授權**單位的金鑰，其值指出 federation SERVICE 的 FQDN，範例 https://fs.contoso.com/adfs/
* 使用您在 AD FS 中新增應用程式群組期間在 [**設定 WEB api** ] 頁面中指定的 web api 識別碼值，修改金鑰**ida：物件**。
* 新增金鑰**ida： AdfsMetadataEndpoint** ，其值對應至 AD FS 服務的同盟中繼資料 URL，例如： https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

![Web 設定](media/native-client-with-ad-fs-2016/webconfig.PNG)


**Startup.Auth.cs**

修改 Configureauth ... 函數，如下所示

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

基本上，我們會將驗證設定為使用 AD FS 並進一步提供 AD FS 中繼資料的相關資訊，並驗證權杖，物件宣告應該是 Web API 所預期的值。
執行應用程式

1. 在 方案 Nativeclient-android-DotNet 上，以滑鼠右鍵按一下並移至 屬性。 如下所示，將啟始專案變更為多個啟始專案，並將 TodoListClient 和 TodoListService 都設定為 [啟動]。
![解決方案屬性](media/native-client-with-ad-fs-2016/solutionproperties.png)

2.  按下 F5 鍵，或選取功能表列中的 [Debug > 繼續]。 這會同時啟動原生應用程式和 WebAPI。 在原生應用程式上按一下 [登入] 按鈕，它會彈出來自 AD AL 的互動式登入，然後重新導向至您的 AD FS 服務。 輸入有效使用者的認證。
![登入](media/native-client-with-ad-fs-2016/sign-in.png)

在此步驟中，原生應用程式會重新導向至 AD FS 並取得 Web API 的識別碼權杖和存取權杖。

3. 在文字方塊中輸入待辦事項，然後按一下 [加入專案]。 在此步驟中，應用程式會前往 Web API 來新增待辦事項專案，若要這麼做，請將存取權杖提供給從 AD FS 取得的 WebAPI。 Web API 會符合物件的值，以確定權杖適用于它，並使用來自同盟中繼資料的資訊來驗證權杖簽章。

![登入](media/native-client-with-ad-fs-2016/clienttodoadd.png)

## <a name="next-steps"></a>後續步驟
[AD FS 開發](../../ad-fs/AD-FS-Development.md)  
