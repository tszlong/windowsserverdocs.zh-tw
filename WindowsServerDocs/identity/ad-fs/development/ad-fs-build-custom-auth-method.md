---
title: 在 Windows Server 中建立 AD FS 的自訂驗證方法
description: 此案例說明如何在 Windows Server 中建立 AD FS 的自訂驗證方法。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2ef16ddeb241d55b61b484805ff91cb247985d8d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358879"
---
# <a name="build-a-custom-authentication-method-for-ad-fs-in-windows-server"></a>在 Windows Server 中建立 AD FS 的自訂驗證方法

本逐步解說提供在 Windows Server 2012 R2 中為 AD FS 執行自訂驗證方法的指示。 如需詳細資訊，請參閱[其他驗證方法](https://msdn.microsoft.com/library/dn758113\(v=msdn.10\))。


> [!WARNING]
> 您可以在這裡&nbsp;建立的範例僅供教育目的之用。 &nbsp;這些指示適用于公開模型所需元素的最簡單且最基本的執行方式。&nbsp;沒有驗證後端、錯誤處理或設定資料。 
> <P></P>



## <a name="setting-up-the-development-box"></a>設定開發箱

本逐步解說會使用 Visual Studio 2012。  專案可以使用任何開發環境來建立，以建立適用于 Windows 的 .NET 類別。 專案必須以 .NET 4.5 為目標，因為**BeginAuthentication**和**TryEndAuthentication**方法會使用 .NET Framework 4.5 版的**元件。專案**需要一個參考：


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>參考 dll</strong></p></td>
<td><p><strong>尋找位置</strong></p></td>
<td><p><strong>需要</strong></p></td>
</tr>
<tr class="even">
<td><p>IdentityServer. Web .dll</p></td>
<td><p>Dll 位於已安裝 AD FS 之 Windows Server 2012 R2 伺服器上的%windir%\ADFS 中。</p>
<p></p>
<p>此 dll 必須複製到開發電腦，以及在專案中建立的明確參考。</p></td>
<td><p>介面類別型，包括 IAuthenticationCoNtext、IProofData</p></td>
</tr>
</tbody>
</table>


## <a name="create-the-provider"></a>建立提供者

1.  在 Visual Studio 2012：選擇檔案-\>新增-\>專案 。

2.  選取 [類別庫]，並確定您的目標是 .NET 4.5。

    ![建立提供者](media/ad-fs-build-custom-auth-method/Dn783423.71a57ae1-d53d-462b-a846-5b3c02c7d3f2(MSDN.10).jpg "建立提供者")

3.  在已安裝 AD FS 的 Windows Server 2012 R2 伺服器上，從% windir% \\ADFS 複製**IdentityServer** ，並將它貼入開發電腦上的專案資料夾中。

4.  在**方案總管**中，以滑鼠右鍵按一下 [**參考**] 並**新增參考 ...**

5.  流覽至您的**IdentityServer**本機複本，然後**新增 ...**

6.  按一下 **[確定]** 以確認新的參考：

    ![建立提供者](media/ad-fs-build-custom-auth-method/Dn783423.f18df353-9259-4744-b4b6-dd780ce90951(MSDN.10).jpg "建立提供者")

    您現在應該設定為解析提供者所需的所有類型。 

7.  將新類別新增至您的專案（以滑鼠右鍵按一下您的專案，然後**新增 。類別 ...** ）並提供名稱，例如**MyAdapter**，如下所示：

    ![建立提供者](media/ad-fs-build-custom-auth-method/Dn783423.6b6a7a8b-9d66-40c7-8a86-a2e3b9e14d09(MSDN.10).jpg "建立提供者")

8.  在 [新增檔案] MyAdapter.cs 中，將現有的程式碼取代為下列內容：

        using System;
         using System.Collections.Generic;
         using System.Linq;
         using System.Text;
         using System.Threading.Tasks;
         using System.Globalization;
         using System.IO;
         using System.Net;
         using System.Xml.Serialization;
         using Microsoft.IdentityServer.Web.Authentication.External;
         using Claim = System.Security.Claims.Claim;

         namespace MFAadapter
         {
         class MyAdapter : IAuthenticationAdapter
         {

         }
         }

    現在您應該能夠在 IAuthenticationAdapter 上按 F12 （以滑鼠右鍵按一下 [移至定義]），以查看所需的介面成員集合。 

    接下來，您可以執行這些動作的簡單部署。

9.  以下列內容取代您的類別的完整內容：

        namespace MFAadapter
         {
         class MyAdapter : IAuthenticationAdapter
         {
         public IAuthenticationAdapterMetadata Metadata
         {
         //get { return new <instance of IAuthenticationAdapterMetadata derived class>; }
         }

         public IAdapterPresentation BeginAuthentication(Claim identityClaim, HttpListenerRequest request, IAuthenticationContext authContext)
         {
         //return new instance of IAdapterPresentationForm derived class

         }

         public bool IsAvailableForUser(Claim identityClaim, IAuthenticationContext authContext)
         {
         return true; //its all available for now

         }

         public void OnAuthenticationPipelineLoad(IAuthenticationMethodConfigData configData)
         {
         //this is where AD FS passes us the config data, if such data was supplied at registration of the adapter

         }

         public void OnAuthenticationPipelineUnload()
         {

         }

         public IAdapterPresentation OnError(HttpListenerRequest request, ExternalAuthenticationException ex)
         {
         //return new instance of IAdapterPresentationForm derived class

         }

         public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
         {
         //return new instance of IAdapterPresentationForm derived class

         }

         }
         }

10. 我們尚未準備好建立 。還有兩個介面可以繼續。

    將兩個類別新增至您的專案：一個用於中繼資料，另一個用於簡報表單。  您可以在與上述類別相同的檔案中新增這些檔案。

        class MyMetadata : IAuthenticationAdapterMetadata
         {

         }

         class MyPresentationForm : IAdapterPresentationForm
         {

         }

11. 接下來，您可以為每個新增必要的成員。首先，中繼資料（具有有用的內嵌批註）

        class MyMetadata : IAuthenticationAdapterMetadata
         {
         //Returns the name of the provider that will be shown in the AD FS management UI (not visible to end users)
         public string AdminName
         {
         get { return "My Example MFA Adapter"; }
         }

         //Returns an array of strings containing URIs indicating the set of authentication methods implemented by the adapter 
         /// AD FS requires that, if authentication is successful, the method actually employed will be returned by the
         /// final call to TryEndAuthentication(). If no authentication method is returned, or the method returned is not
         /// one of the methods listed in this property, the authentication attempt will fail.
         public virtual string[] AuthenticationMethods 
         {
         get { return new[] { "http://example.com/myauthenticationmethod1", "http://example.com/myauthenticationmethod2" }; }
         }

         /// Returns an array indicating which languages are supported by the provider. AD FS uses this information
         /// to determine the best language\locale to display to the user.
         public int[] AvailableLcids
         {
         get
         {
         return new[] { new CultureInfo("en-us").LCID, new CultureInfo("fr").LCID};
         }
         }

         /// Returns a Dictionary containing the set of localized friendly names of the provider, indexed by lcid. 
         /// These Friendly Names are displayed in the "choice page" offered to the user when there is more than 
         /// one secondary authentication provider available.
         public Dictionary<int, string> FriendlyNames
         {
         get
         {
         Dictionary<int, string> _friendlyNames = new Dictionary<int, string>();
         _friendlyNames.Add(new CultureInfo("en-us").LCID, "Friendly name of My Example MFA Adapter for end users (en)");
         _friendlyNames.Add(new CultureInfo("fr").LCID, "Friendly name translated to fr locale");
         return _friendlyNames;
         }
         }

         /// Returns a Dictionary containing the set of localized descriptions (hover over help) of the provider, indexed by lcid. 
         /// These descriptions are displayed in the "choice page" offered to the user when there is more than one 
         /// secondary authentication provider available.
         public Dictionary<int, string> Descriptions
         {
         get 
         {
         Dictionary<int, string> _descriptions = new Dictionary<int, string>();
         _descriptions.Add(new CultureInfo("en-us").LCID, "Description of My Example MFA Adapter for end users (en)");
         _descriptions.Add(new CultureInfo("fr").LCID, "Description translated to fr locale");
         return _descriptions; 
         }
         }

         /// Returns an array indicating the type of claim that the adapter uses to identify the user being authenticated.
         /// Note that although the property is an array, only the first element is currently used.
         /// MUST BE ONE OF THE FOLLOWING
         /// "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"
         /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
         /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"
         /// "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"
         public string[] IdentityClaims
         {
         get { return new[] { "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" }; }
         }

         //All external providers must return a value of "true" for this property.
         public bool RequiresIdentity
         {
         get { return true; }
         }
        }

    接下來，簡報的形式如下：

        class MyPresentationForm : IAdapterPresentationForm
         {
         /// Returns the HTML Form fragment that contains the adapter user interface. This data will be included in the web page that is presented
         /// to the cient.
         public string GetFormHtml(int lcid)
         {
         string htmlTemplate = Resources.FormPageHtml; //todo we will implement this
         return htmlTemplate;
         }

         /// Return any external resources, ie references to libraries etc., that should be included in 
         /// the HEAD section of the presentation form html. 
         public string GetFormPreRenderHtml(int lcid)
         {
         return null;
         }

         //returns the title string for the web page which presents the HTML form content to the end user
         public string GetPageTitle(int lcid)
         {
         return "MFA Adapter";
         }


~~~
     }
~~~

12. 請注意上述**FormPageHtml**元素的 ' todo '。 

   您可以在一分鐘內修正此問題，但首先我們要根據新實的型別，將最後必要的 return 語句加入至您的初始 MyAdapter 類別。  若要這樣做，請*將下方的*專案新增至您現有的 IAuthenticationAdapter 執行：

       類別 MyAdapter：IAuthenticationAdapter {public IAuthenticationAdapterMetadata 中繼資料 {//get {return <instance of IAuthenticationAdapterMetadata derived class>new;}    get {return new MyMetadata （）;}    }

        public IAdapterPresentation BeginAuthentication(Claim identityClaim, HttpListenerRequest request, IAuthenticationContext authContext)
        {
        //return new instance of IAdapterPresentationForm derived class
        return new MyPresentationForm();
        }

        public bool IsAvailableForUser(Claim identityClaim, IAuthenticationContext authContext)
        {
        return true; //its all available for now
        }

        public void OnAuthenticationPipelineLoad(IAuthenticationMethodConfigData configData)
        {
        //this is where AD FS passes us the config data, if such data was supplied at registration of the adapter

        }

        public void OnAuthenticationPipelineUnload()
        {

        }

        public IAdapterPresentation OnError(HttpListenerRequest request, ExternalAuthenticationException ex)
        {
        //return new instance of IAdapterPresentationForm derived class
        return new MyPresentationForm();
        }

        public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
        {
        //return new instance of IAdapterPresentationForm derived class
        outgoingClaims = new Claim[0];
        return new MyPresentationForm();
        }

        }

13. 現在是包含 html 片段的資源檔。 在您的專案資料夾中，使用下列內容建立新的文字檔：

       <div id="loginArea">
        <form method="post" id="loginForm" >
        <!-- These inputs are required by the presentation framework. Do not modify or remove -->
        <input id="authMethod" type="hidden" name="AuthMethod" value="%AuthMethod%"/>
        <input id="context" type="hidden" name="Context" value="%Context%"/>
        <!-- End inputs are required by the presentation framework. -->
        <p id="pageIntroductionText">此內容是由 MFA 範例介面卡提供。 以下顯示挑戰輸入。</p>
        <label for="challengeQuestionInput" class="block">Question text @ no__t-1 @ no__t-2 @ no__t-3<div id="submissionArea" class="submitMargin">
        <input id="submitButton" type="submit" name="Submit" value="Submit" onclick="return AuthPage.submitAnswer()"/>
        </div>
        </form>
        <div id="intro" class="groupMargin">
        <p id="supportEmail">支援資訊</p>
        </div>
        <script type="text/javascript" language="JavaScript">
        //<![CDATA[
        function AuthPage() { }
        AuthPage.submitAnswer = function () { return true; };
        //]]>
        </script></div>

14. 然後，選取 **[專案\>-新增元件 ...]資源**檔並將檔案命名為**資源**，然後按一下 [**新增]：**

   ![建立提供者](media/ad-fs-build-custom-auth-method/Dn783423.3369ad8f-f65f-4f36-a6d5-6a3edbc1911a(MSDN.10).jpg "建立提供者")

15. 然後，在**Resources .resx**檔案中，選擇 [**加入資源]。新增現有**的檔案。  流覽至您在上方儲存的文字檔（包含 html 片段）。

   請確定您的 GetFormHtml 程式碼會由資源檔（.resx 檔案）名稱前置詞正確解析新資源的名稱，後面接著資源本身的名稱：

       public string GetFormHtml （int lcid） {string htmlTemplate = Resources. MfaFormHtml;//Resxfilename.resourcename return htmlTemplate;   }

   您現在應該可以建立。

## <a name="build-the-adapter"></a>建立介面卡

介面卡必須內建在強式名稱的 .NET 元件中，才能安裝到 Windows 的 GAC 中。  若要在 Visual Studio 專案中達成此目的，請完成下列步驟：

1.  在方案總管中，以滑鼠右鍵按一下您的專案名稱，然後按一下 [**屬性**]。

2.  在 [**簽署**] 索引標籤上，核取 **[簽署元件**]，然後選擇 [ **\<新增 ...]在\>** **[選擇強式名稱金鑰**檔] 底下：輸入金鑰檔名稱和密碼，然後按一下 **[確定]** 。  然後，確定已核取 **[簽署元件**]，而且未選取 [**延遲簽**章]。  [屬性**簽署**] 頁面看起來應該像這樣：

    ![建立提供者](media/ad-fs-build-custom-auth-method/Dn783423.0b1a1db2-d64e-4bb8-8c01-ef34296a2668(MSDN.10).jpg "建立提供者")

3.  然後建立解決方案。

## <a name="deploy-the-adapter-to-your-ad-fs-test-machine"></a>將介面卡部署至您的 AD FS 測試電腦

您必須先在系統中註冊外部提供者，才能由 AD FS 叫用它。  介面卡提供者必須提供執行必要安裝動作的安裝程式，包括安裝在 GAC 中，而且安裝程式必須支援 AD FS 中的註冊。  如果尚未完成，則系統管理員必須執行下列 Windows PowerShell 步驟。  這些步驟可以在實驗室中用來啟用測試和調試。

### <a name="prepare-the-test-ad-fs-machine"></a>準備測試 AD FS 機

複製檔案，並將其新增至 GAC。

1.  確定您有 Windows Server 2012 R2 電腦或虛擬機器。

2.  安裝 AD FS 角色服務，並使用至少一個節點來設定伺服器陣列。

    如需在實驗室環境中設定同盟伺服器的詳細步驟，請參閱《 [Windows server 2012 R2 AD FS 部署指南》](https://msdn.microsoft.com/library/dn486820\(v=msdn.10\))。

3.  將 Gacutil 工具複製到伺服器。

    Gacutil 位於 **% homedrive% \\Program Files （x86） \\Microsoft sdk @ no__t-3Windows @ no__t-4v 8.0 a @ no__t-5bin @ no__t-6NETFX 4.0 Tools @ no__t-7** （在 Windows 8 電腦上）。  您需要**gacutil**檔案本身以及**1033**、 **en-us**和其他當地語系化資源資料夾，位於**NETFX 4.0 工具**位置底下。

4.  將您的提供者檔案（一個或多個強式名稱簽署的 .dll 檔案）複製到與**gacutil**相同的資料夾位置（此位置只是為了方便起見）

5.  將您的 .dll 檔案加入至伺服器陣列中每部 AD FS 同盟伺服器上的 GAC：

    範例：使用命令列工具 GACutil 將 dll 新增至 GAC：`C:\>.\gacutil.exe /if .\<yourdllname>.dll`

    若要在 GAC 中查看產生的專案：`C:\>.\gacutil.exe /l <yourassemblyname>`

6.  

### <a name="register-your-provider-in-ad-fs"></a>在 AD FS 中註冊您的提供者

符合上述先決條件之後，請在您的同盟伺服器上開啟 Windows PowerShell 命令視窗，並輸入下列命令（請注意，如果您使用的是使用 Windows Internal Database 的同盟伺服器陣列，您必須在下列電腦上執行這些命令：伺服器陣列的主要同盟伺服器）：

1.  `Register-AdfsAuthenticationProvider –TypeName YourTypeName –Name “AnyNameYouWish” [–ConfigurationFilePath (optional)]`

    其中 YourTypeName 是您的 .NET 強型別名稱："YourDefaultNamespace. YourIAuthenticationAdapterImplementationClassName，YourAssemblyName，Version = YourAssemblyVersion，Culture = 中性，PublicKeyToken = YourPublicKeyTokenValue，processorArchitecture = MSIL"

    這會在 AD FS 中註冊您的外部提供者，並以您在上面所提供的名稱 AnyNameYouWish。

2.  重新開機 AD FS 服務（例如，使用 Windows 服務嵌入式管理單元）。

3.  執行下列命令: `Get-AdfsAuthenticationProvider`。

    這會將您的提供者顯示為系統中的其中一個提供者。

    範例：

        PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”
        PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter”
        PS C:\>net stop adfssrv
        PS C:\>net start adfssrv

    如果您已在 AD FS 環境中啟用裝置註冊服務，請同時執行下列步驟：`PS C:\>net start drs`

    若要確認已註冊的提供者，請使用`PS C:\>Get-AdfsAuthenticationProvider`下列命令：。

    這會將您的提供者顯示為系統中的其中一個提供者。

### <a name="create-the-ad-fs-authentication-policy-that-invokes-your-adapter"></a>建立用來叫用介面卡的 AD FS 驗證原則

#### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>使用 AD FS 管理嵌入式管理單元建立驗證原則

1.  開啟 [AD FS 管理] 嵌入式管理單元（從 [伺服器管理員**工具**] 功能表）。

2.  按一下 [**驗證原則**]。

3.  在中央窗格的 [**多重要素驗證**] 底下，按一下 [**全域設定**] 右邊的 [**編輯**] 連結。

4.  在頁面底部的 [**選取其他驗證方法**] 底下，選取提供者 AdminName 的方塊。 按一下 **[套用]** 。

5.  若要提供「觸發程式」以使用介面卡叫用 MFA，請在 [**位置**] 底下檢查**外部**網路和**內部**網路，例如。 按一下 [確定]。 （若要設定每個信賴憑證者的觸發程式，請參閱下方的「使用 Windows PowerShell 建立驗證原則」）。

6.  使用下列命令來檢查結果：

    第一`Get-AdfsGlobalAuthenticationPolicy`次使用。 您應該會看到提供者名稱做為其中一個 AdditionalAuthenticationProvider 值。

    然後使用`Get-AdfsAdditionalAuthenticationRule`。 您應該會看到外部網路和內部網路的規則，設定為系統管理員 UI 中的原則選取結果。

#### <a name="create-the-authentication-policy-using-windows-powershell"></a>使用 Windows PowerShell 建立驗證原則

1.  首先，在全域原則中啟用提供者：

    `PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider “YourAuthProviderName”`


~~~
> [!NOTE]
> Note that the value provided for the AdditionalAuthenticationProvider parameter corresponds to the value you provided for the “Name” parameter in the Register-AdfsAuthenticationProvider cmdlet above and to the “Name” property from Get-AdfsAuthenticationProvider cmdlet output. 
> <P></P>


Example:`PS C:\>Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider “MyMFAAdapter”`
~~~

2. 接下來，設定全域或信賴憑證者特定規則以觸發 MFA：

   範例1：若要建立全域規則以要求外部要求的 MFA：`PS C:\>Set-AdfsAdditionalAuthenticationRule –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'`

   範例2：建立 MFA 規則，要求對特定信賴憑證者的外部要求進行 MFA。  （請注意，個別提供者無法連線到 Windows Server 2012 R2 AD FS 中的個別信賴憑證者。

       PS C:\>$rp = Get-AdfsRelyingPartyTrust –Name <Relying Party Name>
       PS C:\>Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'

### <a name="authenticate-with-mfa-using-your-adapter"></a>使用您的介面卡向 MFA 進行驗證

最後，執行下列步驟來測試您的介面卡：

1.  請確定 AD FS 全域主要驗證類型已設定為外部網路和內部網路的表單驗證（這可讓您更輕鬆地以特定使用者的身分進行驗證）

    1.  在 [AD FS] 嵌入式管理單元中，于 [**驗證原則**] 下的 [**主要驗證**] 區域中，按一下 [**全域設定**] 旁的 [**編輯**]。

        1.  或者，只需按一下 [**多重要素原則**] UI 中的 [**主要**] 索引標籤。

2.  確定 [**表單驗**證] 是同時檢查外部網路和內部網路驗證方法的唯一選項。  按一下 [確定]。

3.  開啟 IDP 起始的登入 html 頁面（HTTPs://\<fsname\>/adfs/ls/iDPInitiatedsignon.htm），然後在您的測試環境中以有效的 AD 使用者身分登入。

4.  輸入認證以進行主要驗證。

5.  您應該會看到包含範例挑戰問題的 [MFA 表單] 頁面。 

    如果您已設定多個介面卡，您會看到 [MFA 選擇] 頁面上有您的易記名稱。

    ![使用介面卡進行驗證](media/ad-fs-build-custom-auth-method/Dn783423.c98d2712-cbd3-4cb9-ac03-2838b81c4f63(MSDN.10).jpg "使用介面卡進行驗證")

    ![使用介面卡進行驗證](media/ad-fs-build-custom-auth-method/Dn783423.fd3aefc0-ef6c-4a8c-a737-4914c78ff2d2(MSDN.10).jpg "使用介面卡進行驗證")

您現在已可正常執行介面，並瞭解模型的運作方式。 您可以 trym 為額外的範例，以在 BeginAuthentication 和 TryEndAuthentication 中設定中斷點。  請注意，當使用者第一次進入 MFA 表單時，BeginAuthentication 的執行方式，而 TryEndAuthentication 則是在每次提交表單時觸發。

## <a name="update-the-adapter-for-successful-authentication"></a>更新介面卡以驗證成功

等待–您的範例介面卡永遠不會成功驗證\!  這是因為您程式碼中的任何內容都不會針對 TryEndAuthentication 傳回 null。

完成上述程式之後，您建立了基本介面卡的執行，並將它新增至 AD FS 伺服器。  您可以取得 [MFA 表單] 頁面，但您還無法驗證，因為您尚未將正確的邏輯放在 TryEndAuthentication 的執行中。  讓我們來加入它。

回想一下您的 TryEndAuthentication 實施：

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     //return new instance of IAdapterPresentationForm derived class
     outgoingClaims = new Claim[0];
     return new MyPresentationForm();

     }

讓我們更新它，讓它不會一律傳回 MyPresentationForm （）。  為此，您可以在類別中建立一個簡單的公用程式方法：

    static bool ValidateProofData(IProofData proofData, IAuthenticationContext authContext)
     {
     if (proofData == null || proofData.Properties == null || !proofData.Properties.ContainsKey("ChallengeQuestionAnswer"))
     {
     throw new ExternalAuthenticationException("Error - no answer found", authContext);
     }

     if ((string)proofData.Properties["ChallengeQuestionAnswer"] == "adfabric")
     {
     return true;
     }
     else
     {
     return false;
     }
     }

然後，更新 TryEndAuthentication，如下所示：

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     outgoingClaims = new Claim[0];
     if (ValidateProofData(proofData, authContext))
     {
     //authn complete - return authn method
     outgoingClaims = new[] 
     {
     // Return the required authentication method claim, indicating the particulate authentication method used.
     new Claim( "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", 
     "http://example.com/myauthenticationmethod1" )
     };
     return null;
     }
     else
     {
     //authentication not complete - return new instance of IAdapterPresentationForm derived class
     return new MyPresentationForm();
     }
     }

現在您必須更新 [測試] 方塊上的介面卡。  您必須先復原 AD FS 原則，然後從 AD FS 取消註冊並重新啟動 AD FS，然後從 GAC 中移除 .dll，然後將新的 .dll 新增至 GAC，然後在 AD FS 中進行註冊，再重新開機 AD FS，然後重新設定 AD FS 原則。

## <a name="deploy-and-configure-the-updated-adapter-on-your-test-ad-fs-machine"></a>在測試 AD FS 機上部署和設定更新的介面卡

### <a name="clear-ad-fs-policy"></a>清除 AD FS 原則

清除 MFA UI 中所有 MFA 相關的核取方塊，如下所示，然後按一下 [確定]。

![清除原則](media/ad-fs-build-custom-auth-method/Dn783423.c111b4e7-5b05-413c-8b0f-222a0e91ac1f(MSDN.10).jpg "清除原則")

### <a name="unregister-provider-windows-powershell"></a>取消註冊提供者（Windows PowerShell）

`PS C:\> Unregister-AdfsAuthenticationProvider –Name “YourAuthProviderName”`

實例`PS C:\> Unregister-AdfsAuthenticationProvider –Name “MyMFAAdapter”`

請注意，您為「名稱」傳遞的值與您提供給 Register-adfsauthenticationprovider 指令程式的「名稱」值相同。  它也是 Register-adfsauthenticationprovider 所輸出的 "Name" 屬性。

請注意，取消註冊提供者之前，您必須先從 AdfsGlobalAuthenticationPolicy 中移除提供者（藉由清除您在 AD FS 管理嵌入式管理單元中簽入的核取方塊，或使用 Windows PowerShell）。

請注意，在這種作業之後，必須重新開機 AD FS 服務。

### <a name="remove-assembly-from-gac"></a>從 GAC 移除元件

1.  首先，使用下列命令來尋找專案的完整強式名稱：`C:\>.\gacutil.exe /l <yourAdapterAssemblyName>`

    實例`C:\>.\gacutil.exe /l mfaadapter`

2.  然後，使用下列命令從 GAC 中移除它：`.\gacutil /u “<output from the above command>”`

    實例`C:\>.\gacutil /u “mfaadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

### <a name="add-the-updated-assembly-to-gac"></a>將更新的元件加入至 GAC

請務必先在本機貼上更新的 .dll。 `C:\>.\gacutil.exe /if .\MFAAdapter.dll`

### <a name="view-assembly-in-the-gac-cmd-line"></a>GAC 中的視圖元件（cmd 行）

`C:\> .\gacutil.exe /l mfaadapter`

### <a name="register-your-provider-in-ad-fs"></a>在 AD FS 中註冊您的提供者

1.  `PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.1, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

2.  `PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter1”`

3.  重新開機 AD FS 服務。

### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>使用 AD FS 管理嵌入式管理單元建立驗證原則

1.  開啟 [AD FS 管理] 嵌入式管理單元（從 [伺服器管理員**工具**] 功能表）。

2.  按一下 [**驗證原則**]。

3.  在 [**多重要素驗證**] 底下，按一下 [**全域設定**] 右邊的 [**編輯**] 連結。

4.  在 [**選取其他驗證方法**] 底下，勾選提供者 AdminName 的方塊。 按一下 **[套用]** 。

5.  若要提供「觸發程式」以使用介面卡叫用 MFA，請在 [位置] 底下檢查**外部**網路和**內部**網路，例如。 按一下 [確定]。

### <a name="authenticate-with-mfa-using-your-adapter"></a>使用您的介面卡向 MFA 進行驗證

最後，執行下列步驟來測試您的介面卡：

1.  請確定 AD FS 全域主要驗證類型已設定為外部網路和內部網路的**表單驗證**（這可讓您更輕鬆地以特定使用者的身分進行驗證）。

    1.  在 [AD FS 管理] 嵌入式管理單元中，于 [**驗證原則**] 下的 [**主要驗證**] 區域中，按一下 [**全域設定**] 旁的 [**編輯**]。

        1.  或者，只需按一下 [多重要素原則] UI 中的 [**主要**] 索引標籤。

2.  確定 [**表單驗**證] 是同時檢查**外部**網路和**內部**網路驗證方法的唯一選項。  按一下 [確定]。

3.  開啟 IDP 起始的登入 html 頁面（HTTPs://\<fsname\>/adfs/ls/iDPInitiatedsignon.htm），然後在您的測試環境中以有效的 AD 使用者身分登入。

4.  輸入主要驗證的認證。

5.  您應該會看到 [MFA 表單] 頁面顯示範例挑戰文字。

    1.  如果您已設定多個介面卡，則會看到具有易記名稱的 [MFA 選擇] 頁面。

在 [MFA 驗證] 頁面輸入 "adfabric" 時，您應該會看到成功的登入。

![使用介面卡登入](media/ad-fs-build-custom-auth-method/Dn783423.630d8a91-3bfe-4cba-8acf-03eae21530ee(MSDN.10).jpg "使用介面卡登入")

![使用介面卡登入](media/ad-fs-build-custom-auth-method/Dn783423.c340fa73-f70f-4870-b8dd-07900fea4469(MSDN.10).jpg "使用介面卡登入")

## <a name="see-also"></a>另請參閱

#### <a name="other-resources"></a>其他資源

[其他驗證方法](https://msdn.microsoft.com/library/dn758113\(v=msdn.10\))  
[透過其他多重要素驗證管理機密應用程式的風險](https://msdn.microsoft.com/library/dn280949\(v=msdn.10\))

