---
title: 適用於 Windows Server 中的 AD FS 建立自訂的驗證方法
description: 此案例說明如何在 Windows Server 中的 AD fs 建立自訂的驗證方法。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f28458ed9e781df6eca2478b02fb667d9240ca48
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445298"
---
# <a name="build-a-custom-authentication-method-for-ad-fs-in-windows-server"></a>適用於 Windows Server 中的 AD FS 建立自訂的驗證方法

本逐步解說提供適用於 Windows Server 2012 R2 中的 AD FS 實作自訂驗證方法的指示。 如需詳細資訊，請參閱 <<c0> [ 額外的驗證方法](https://msdn.microsoft.com/en-us/library/dn758113\(v=msdn.10\))。


> [!WARNING]
> 您可以在這裡建置的範例是&nbsp;教育之用。 &nbsp;這些指示是最簡單、 最小實作能夠公開 （expose） 模型的必要項目。&nbsp;沒有驗證的後端、 錯誤處理或設定資料。 
> <P></P>



## <a name="setting-up-the-development-box"></a>設定開發電腦

此逐步解說會使用 Visual Studio 2012。  可以使用任何可以為 Windows 建立.NET 類別的開發環境來建置專案。 因為專案必須以.NET 4.5 為目標**BeginAuthentication**並**TryEndAuthentication**方法會使用型別**System.Security.Claims.Claim**屬於.NETFramework 版本 4.5.There 是一個專案所需的參考：


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>參考 dll</strong></p></td>
<td><p><strong>哪裡可以找到它</strong></p></td>
<td><p><strong>所需的</strong></p></td>
</tr>
<tr class="even">
<td><p>Microsoft.IdentityServer.Web.dll</p></td>
<td><p>Dll 位於 %windir%\ADFS 安裝 AD FS 的 Windows Server 2012 R2 伺服器上。</p>
<p></p>
<p>這個 dll 必須複製到開發電腦並在專案中建立的明確參考。</p></td>
<td><p>介面類型包括 IAuthenticationContext，IProofData</p></td>
</tr>
</tbody>
</table>


## <a name="create-the-provider"></a>建立提供者

1.  在 Visual Studio 2012:選擇 [檔案]-\>New-\>專案...

2.  選取 類別庫，並確定您的目標.NET 4.5。

    ![建立提供者](media/ad-fs-build-custom-auth-method/Dn783423.71a57ae1-d53d-462b-a846-5b3c02c7d3f2(MSDN.10).jpg "建立提供者")

3.  建立一份**Microsoft.IdentityServer.Web.dll** %windir%從\\ADFS，AD FS 已安裝，且將它貼在您的專案資料夾，在您的開發電腦上的 Windows Server 2012 R2 伺服器上。

4.  在 [**方案總管] 中**，以滑鼠右鍵按一下**參考**和**加入參考...**

5.  瀏覽至您的本機複本**Microsoft.IdentityServer.Web.dll**和**加入...**

6.  按一下 **確定**確認新的參考：

    ![建立提供者](media/ad-fs-build-custom-auth-method/Dn783423.f18df353-9259-4744-b4b6-dd780ce90951(MSDN.10).jpg "建立提供者")

    您應該現在是設定來解決所有提供者所需的類型。 

7.  將新類別新增至您的專案 (以滑鼠右鍵按一下您的專案，**新增。...類別...** ) 並為它提供的名稱，例如**MyAdapter**，如下所示：

    ![建立提供者](media/ad-fs-build-custom-auth-method/Dn783423.6b6a7a8b-9d66-40c7-8a86-a2e3b9e14d09(MSDN.10).jpg "建立提供者")

8.  在新的檔案 MyAdapter.cs，取代現有的程式碼取代為下列：

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

    現在您應該能夠使用 F12 （以滑鼠右鍵按一下-移至定義） 上 IAuthenticationAdapter，若要查看必要的介面成員的集合。 

    接下來，您可以執行這些簡單的實作。

9.  您的類別的整個內容取代下列：

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

10. 不，我們已準備好尚未建置...有兩個的多個介面，前往。

    專案中加入兩個的多個類別： 一個適用於中繼資料，和另一個則用於呈現表單。  您可以將這些相同的檔案中為上述的類別。

        class MyMetadata : IAuthenticationAdapterMetadata
         {

         }

         class MyPresentationForm : IAdapterPresentationForm
         {

         }

11. 接下來，您可以新增每個必要的成員。首先，中繼資料 （很有幫助的內嵌註解）

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

         /// Returns an array indicating the type of claim that that the adapter uses to identify the user being authenticated.
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

    下一步，呈現表單：

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

12. 請注意 'todo' **Resources.FormPageHtml**上述項目。 

   您可以修正此問題在分鐘，但第一個讓我們新增最後一個必要的 return 陳述式，根據您初始的 MyAdapter 類別的新實作型別。  若要這樣做，請新增 中的項目*斜體*下方以您現存的 IAuthenticationAdapter 實作：

       類別 MyAdapter:IAuthenticationAdapter     {     public IAuthenticationAdapterMetadata Metadata     {     //get { return new <instance of IAuthenticationAdapterMetadata derived class>; }     get { return new MyMetadata(); }     }

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

13. 現在，為資源檔包含的 html 片段。 在您的專案資料夾，使用下列內容建立新的文字檔：

       <div id="loginArea">
        <form method="post" id="loginForm" >
        <!-- These inputs are required by the presentation framework. Do not modify or remove -->
        <input id="authMethod" type="hidden" name="AuthMethod" value="%AuthMethod%"/>
        <input id="context" type="hidden" name="Context" value="%Context%"/>
        <!-- End inputs are required by the presentation framework. -->
        <p id="pageIntroductionText">MFA 範例配接器會提供此內容。 挑戰的輸入應該顯示如下。</p>
        <label for="challengeQuestionInput" class="block">問題文字</label>
        <input id="challengeQuestionInput" name="ChallengeQuestionAnswer" type="text" value="" class="text" placeholder="Answer placeholder" />
        <div id="submissionArea" class="submitMargin">
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

14. 然後，選取**專案-\>新增元件...資源**檔案，並將檔案命名**資源**，然後按一下**新增：**

   ![建立提供者](media/ad-fs-build-custom-auth-method/Dn783423.3369ad8f-f65f-4f36-a6d5-6a3edbc1911a(MSDN.10).jpg "建立提供者")

15. 然後，在**Resources.resx**檔案中，選擇**加入資源...將現有的檔案**。  瀏覽至文字檔案 （包含 html 片段） 您儲存上方。

   請確定您 GetFormHtml 的程式碼的新資源的名稱正確解析由資源檔 （.resx 檔案） 名稱前置詞後面接著資源本身的名稱：

       public string GetFormHtml(int lcid)    {     string htmlTemplate = Resources.MfaFormHtml; //Resxfilename.resourcename     return htmlTemplate;    }

   您現在應該能夠進行建置。

## <a name="build-the-adapter"></a>建置配接器

配接器應建立強式名稱的.NET 組件安裝到 GAC 中，在 Windows 中。  若要在 Visual Studio 專案中這麼做，請完成下列步驟：

1.  以滑鼠右鍵按一下您的專案名稱，在 方案總管 中，按一下 **屬性**。

2.  在上**簽署**索引標籤上，勾選**簽署組件**，然後選擇  **\<新增...\>** 下方**選擇強式名稱金鑰檔：** 輸入的金鑰檔名稱和密碼，然後按一下**確定**。  請確定**簽署組件**會檢查並**僅延遲簽署**未核取。  屬性**簽署**頁面應該看起來像這樣：

    ![建置提供者](media/ad-fs-build-custom-auth-method/Dn783423.0b1a1db2-d64e-4bb8-8c01-ef34296a2668(MSDN.10).jpg "建置提供者")

3.  然後建置方案。

## <a name="deploy-the-adapter-to-your-ad-fs-test-machine"></a>部署到您的 AD FS 測試機器的配接器

AD FS 可以叫用外部提供者之前，它必須在系統中註冊。  配接器提供者必須提供執行必要的安裝動作，包括安裝在 GAC 中，安裝程式，安裝程式必須在 AD FS 中支援註冊。  如果未完成，系統管理員必須執行下列 Windows PowerShell 步驟。  這些步驟可以用於啟用偵錯和測試實驗室。

### <a name="prepare-the-test-ad-fs-machine"></a>準備測試 AD FS 電腦

複製檔案，並新增至 GAC。

1.  請確定您已在 Windows Server 2012 R2 的電腦或虛擬機器。

2.  安裝 AD FS 角色服務，並設定至少一個節點伺服器陣列。

    設定實驗室環境中的同盟伺服器的詳細步驟，請參閱[Windows Server 2012 R2 AD FS 部署指南](https://msdn.microsoft.com/en-us/library/dn486820\(v=msdn.10\))。

3.  Gacutil.exe 將工具複製到伺服器。

    Gacutil.exe 位於 **%homedrive%\\Program Files (x86)\\Microsoft SDKs\\Windows\\v8.0A\\bin\\NETFX 4.0 工具\\** 在 Windows 8 電腦上。  您必須**gacutil.exe**檔案本身，以及**1033年**， **EN-US**，以及下列的其他當地語系化的資源資料夾**NETFX 4.0 工具**位置。

4.  將您的提供者檔案複製 （一或多個強式名稱簽署.dll 檔案） 至相同的資料夾位置**gacutil.exe** （只是為了方便起見是位置）

5.  加入伺服器陣列中每個 AD FS 同盟伺服器上的 GAC 中的.dll 檔案：

    範例： 使用命令列工具 GACutil.exe 將 dll 新增至 GAC: `C:\>.\gacutil.exe /if .\<yourdllname>.dll`

    若要在 GAC 中檢視產生的項目：`C:\>.\gacutil.exe /l <yourassemblyname>`

6.  

### <a name="register-your-provider-in-ad-fs"></a>在 AD FS 中註冊您的提供者

一旦符合上述必要條件，開啟您的同盟伺服器上的 Windows PowerShell 命令視窗並輸入下列命令 (請注意，是否您使用的使用 Windows Internal Database 的同盟伺服器陣列，您必須執行這些命令在主要同盟伺服器的伺服器陣列）：

1.  `Register-AdfsAuthenticationProvider –TypeName YourTypeName –Name “AnyNameYouWish” [–ConfigurationFilePath (optional)]`

    其中 YourTypeName 是您的.NET 強式型別名稱：「 YourDefaultNamespace.YourIAuthenticationAdapterImplementationClassName，YourAssemblyName，版本 = YourAssemblyVersion，Culture = neutral，PublicKeyToken = YourPublicKeyTokenValue，processorArchitecture = MSIL"

    這會在 AD FS 中，您為 AnyNameYouWish 上面提供的名稱登錄外部提供者。

2.  重新啟動 AD FS 服務 （例如，使用 [Windows 服務] 嵌入式管理單元）。

3.  執行下列命令： `Get-AdfsAuthenticationProvider`。

    這會顯示其中一個提供者為您的提供者在系統中。

    範例：

        PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”
        PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter”
        PS C:\>net stop adfssrv
        PS C:\>net start adfssrv

    如果您有在您的 AD FS 環境中啟用裝置註冊服務時，也執行下列作業︰  `PS C:\>net start drs`

    若要確認已註冊的提供者，請使用下列命令：`PS C:\>Get-AdfsAuthenticationProvider`。

    這會顯示其中一個提供者為您的提供者在系統中。

### <a name="create-the-ad-fs-authentication-policy-that-invokes-your-adapter"></a>建立會叫用您的配接器的 AD FS 驗證原則

#### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>建立使用 AD FS 管理嵌入式管理單元的驗證原則

1.  開啟 AD FS 管理嵌入式管理單元 (從 伺服器管理員**工具**功能表)。

2.  按一下 **驗證原則**。

3.  在中央窗格中，在**Multi-factor Authentication**，按一下**編輯**的右邊連結**全域設定**。

4.  底下**選取其他驗證方法**在頁面底部，核取方塊的提供者的 AdminName。 按一下 **[套用]** 。

5.  若要提供 「 觸發程序 「 若要叫用下使用您的配接器的 MFA**位置**同時核取**外部網路**並**內部網路**，例如。 按一下 [確定]  。 (若要設定觸發程序每個信賴憑證者合作對象資訊，請參閱下方的 [建立驗證原則使用 Windows PowerShell]。)

6.  請使用下列命令的結果：

    第一次使用`Get-AdfsGlobalAuthenticationPolicy`。 您應該會看到您的提供者名稱做為其中一個 AdditionalAuthenticationProvider 值。

    然後使用`Get-AdfsAdditionalAuthenticationRule`。 您應該會看到針對外部網路和內部網路，因為您的原則選取系統管理員 UI 中設定的規則。

#### <a name="create-the-authentication-policy-using-windows-powershell"></a>建立使用 Windows PowerShell 的驗證原則

1.  首先，讓全域原則中的提供者：

    `PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider “YourAuthProviderName”`


~~~
> [!NOTE]
> Note that the value provided for the AdditionalAuthenticationProvider parameter corresponds to the value you provided for the “Name” parameter in the Register-AdfsAuthenticationProvider cmdlet above and to the “Name” property from Get-AdfsAuthenticationProvider cmdlet output. 
> <P></P>


Example:`PS C:\>Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider “MyMFAAdapter”`
~~~

2. 接下來，設定全域或信賴憑證者合作對象特有的規則，以觸發程序 MFA:

   範例 1： 建立要求的外部要求使用 MFA 的通用規則：`PS C:\>Set-AdfsAdditionalAuthenticationRule –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'`

   範例 2： 建立 MFA 規則來要求使用 MFA 進行外部要求特定的信賴憑證者合作對象。  （請注意，個別的提供者無法連接到個別信賴憑證者的合作對象的 Windows Server 2012 R2 AD FS）。

       PS C:\>$rp = Get-AdfsRelyingPartyTrust –Name <Relying Party Name>
       PS C:\>Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'

### <a name="authenticate-with-mfa-using-your-adapter"></a>使用您的配接器的 MFA 進行驗證

最後，執行下列步驟來測試您的配接器：

1.  請確定 AD FS 全域主要驗證類型設定為表單驗證外部網路和內部網路 （這可讓您示範容易特定使用者的身分進行驗證）

    1.  在 AD FS 嵌入式管理單元，在**驗證原則**，請在**主要驗證**區域中，按一下 **編輯**旁**全域設定**.

        1.  或直接按一下**主要**索引標籤上，從**多重要素原則**UI。

2.  請確定**表單驗證**外部網路和內部網路驗證方法檢查唯一的選項。  按一下 [確定]  。

3.  開啟 IDP 初始化登入 html 頁面 (https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm) 和測試環境中有效的 AD 使用者身分登入。

4.  輸入主要驗證認證。

5.  您應該會看到 MFA 表單範例挑戰問題的頁面就會出現。 

    如果您有多張介面卡設定，您會看到 MFA 選擇頁面與您從上面的易記名稱。

    ![使用配接器進行驗證](media/ad-fs-build-custom-auth-method/Dn783423.c98d2712-cbd3-4cb9-ac03-2838b81c4f63(MSDN.10).jpg "向配接器")

    ![使用配接器進行驗證](media/ad-fs-build-custom-auth-method/Dn783423.fd3aefc0-ef6c-4a8c-a737-4914c78ff2d2(MSDN.10).jpg "向配接器")

您現在可以使用介面的實作，而且您具有模型的運作方式的知識。 您可以 trym 做為額外的範例，在 BeginAuthentication 以及 TryEndAuthentication 中設定中斷點。  請注意如何 BeginAuthentication 時所執行的使用者第一次進入 MFA 表單，而 TryEndAuthentication 會在每個送出表單的觸發。

## <a name="update-the-adapter-for-successful-authentication"></a>更新成功驗證的配接器

但等候 – 從未成功驗證您的範例配接器\!  這是因為在您的程式碼中為 nothing TryEndAuthentication 為傳回 null。

藉由完成上述程序，您會建立基本的配接器實作，並將它新增至 AD FS 伺服器。  您可以取得 MFA 表單頁面上，但您不能尚未通過驗證，因為您不尚未置於正確的邏輯 TryEndAuthentication 實作。  因此我將它加入。

您應該記得 TryEndAuthentication 實作：

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     //return new instance of IAdapterPresentationForm derived class
     outgoingClaims = new Claim[0];
     return new MyPresentationForm();

     }

讓我們更新，以便它永遠不會傳回 MyPresentationForm()。  這個中，您可以建立一個簡單的公用程式方法將類別中：

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

接著，更新 TryEndAuthentication，如下所示：

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

現在，您必須更新 [測試] 方塊上的介面卡。  您必須先復原在 AD FS 的原則，然後取消註冊從 AD FS 和重新啟動 AD FS 中，然後從 GAC 中，移除此.dll 檔然後將此新的.dll 檔加入至 GAC 中，然後在 AD FS 中註冊、 重新啟動 AD FS 中，並重新設定 AD FS 原則。

## <a name="deploy-and-configure-the-updated-adapter-on-your-test-ad-fs-machine"></a>部署和設定上測試 AD FS 電腦的已更新的配接器

### <a name="clear-ad-fs-policy"></a>清除 AD FS 原則

清除所有 MFA 相關在 MFA UI 中，如下所示的核取方塊，然後按一下 [確定]。

![清除原則](media/ad-fs-build-custom-auth-method/Dn783423.c111b4e7-5b05-413c-8b0f-222a0e91ac1f(MSDN.10).jpg "清除原則")

### <a name="unregister-provider-windows-powershell"></a>取消註冊提供者 (Windows PowerShell)

`PS C:\> Unregister-AdfsAuthenticationProvider –Name “YourAuthProviderName”`

範例：`PS C:\> Unregister-AdfsAuthenticationProvider –Name “MyMFAAdapter”`

請注意，您可以傳遞 「 名稱 」 是 「 名稱 」 相同的值的值您提供至 Register-adfsauthenticationprovider cmdlet。  這也是輸出從 Get Register-adfsauthenticationprovider"Name"屬性。

請注意，取消註冊提供者之前，您必須移除提供者從 AdfsGlobalAuthenticationPolicy （無論是藉由清除核取方塊，您簽入 AD FS 管理嵌入式管理單元或使用 Windows PowerShell。）

請注意這項作業後，必須重新啟動 AD FS 服務。

### <a name="remove-assembly-from-gac"></a>從 GAC 移除組件

1.  若要尋找完整的強式名稱的項目，首先，使用下列命令：`C:\>.\gacutil.exe /l <yourAdapterAssemblyName>`

    範例：`C:\>.\gacutil.exe /l mfaadapter`

2.  若要從 GAC 中移除，然後使用下列命令：`.\gacutil /u “<output from the above command>”`

    範例：`C:\>.\gacutil /u “mfaadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

### <a name="add-the-updated-assembly-to-gac"></a>已更新組件加入 GAC

請確定您先貼上從本機更新的.dll。 `C:\>.\gacutil.exe /if .\MFAAdapter.dll`

### <a name="view-assembly-in-the-gac-cmd-line"></a>檢視組件在 GAC （命令列）

`C:\> .\gacutil.exe /l mfaadapter`

### <a name="register-your-provider-in-ad-fs"></a>在 AD FS 中註冊您的提供者

1.  `PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.1, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

2.  `PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter1”`

3.  重新啟動 AD FS 服務。

### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>建立使用 AD FS 管理嵌入式管理單元的驗證原則

1.  開啟 AD FS 管理嵌入式管理單元 (從 伺服器管理員**工具**功能表)。

2.  按一下 **驗證原則**。

3.  底下**Multi-factor Authentication**，按一下**編輯**的右邊連結**全域設定**。

4.  底下**選取其他驗證方法**，針對您的提供者 AdminName 核取方塊。 按一下 **[套用]** 。

5.  若要提供 「 觸發程序 「 若要叫用使用配接器的 MFA，位置下同時檢查**外部網路**並**內部網路**，例如。 按一下 [確定]  。

### <a name="authenticate-with-mfa-using-your-adapter"></a>使用您的配接器的 MFA 進行驗證

最後，執行下列步驟來測試您的配接器：

1.  請確定 AD FS 全域主要驗證類型設定為**表單驗證**外部網路和內部網路 （這可讓您更輕鬆地驗證特定使用者的身分）。

    1.  在 AD FS 管理嵌入式管理單元，在**驗證原則**，請在**主要驗證**區域中，按一下**編輯**旁**全域設定**.

        1.  或直接按一下**主要**] 索引標籤，從 [Multi-factor authentication 原則 UI。

2.  請確定**表單驗證**會檢查兩者的唯一選項**外部網路**並**內部網路**驗證方法。  按一下 [確定]  。

3.  開啟 IDP 初始化登入 html 頁面 (https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm) 和測試環境中有效的 AD 使用者身分登入。

4.  輸入的認證進行主要驗證。

5.  您應該會看到 MFA 表單以範例挑戰文字的頁面就會出現。

    1.  如果您有多張介面卡設定，您會看到 MFA 選擇頁面，以您的好記名稱。

輸入 「 adfabric 」，在 [MFA 驗證] 頁面時，您應該看到成功的登入。

![使用配接器登入](media/ad-fs-build-custom-auth-method/Dn783423.630d8a91-3bfe-4cba-8acf-03eae21530ee(MSDN.10).jpg "配接器使用登入")

![使用配接器登入](media/ad-fs-build-custom-auth-method/Dn783423.c340fa73-f70f-4870-b8dd-07900fea4469(MSDN.10).jpg "配接器使用登入")

## <a name="see-also"></a>另請參閱

#### <a name="other-resources"></a>其他資源

[其他驗證方法](https://msdn.microsoft.com/en-us/library/dn758113\(v=msdn.10\))  
[透過機密應用程式的其他多因素驗證管理風險](https://msdn.microsoft.com/en-us/library/dn280949\(v=msdn.10\))

