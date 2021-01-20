---
title: 為 Windows Server 中的 AD FS 建立自訂驗證方法
description: 本案例說明如何在 Windows Server 中建立 AD FS 的自訂驗證方法。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2019
ms.topic: article
ms.openlocfilehash: de69c0f7bf53eefd1da39b2ac629dfe3ccd47668
ms.sourcegitcommit: 7674bbe49517bbfe0e2c00160e08240b60329fd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/20/2021
ms.locfileid: "98603431"
---
# <a name="build-a-custom-authentication-method-for-ad-fs-in-windows-server"></a>為 Windows Server 中的 AD FS 建立自訂驗證方法

本逐步解說提供在 Windows Server 2012 R2 中針對 AD FS 執行自訂驗證方法的指示。 如需詳細資訊，請參閱 [其他驗證方法](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))。

> [!WARNING]
> 您可以在這裡建立的範例 &nbsp; 僅供教育之用。 &nbsp;這些指示是為了公開模型的必要元素，最簡單且最基本的實作為可能。 &nbsp; 沒有任何驗證後端、錯誤處理或設定資料。

## <a name="setting-up-the-development-box"></a>設定開發箱

本逐步解說使用 Visual Studio 2012。 您可以使用任何可建立適用于 Windows 的 .NET 類別的開發環境，建立專案。 專案必須以 .NET 4.5 為目標，因為 BeginAuthentication 和 **TryEndAuthentication** 方法會使用和方法，這是 .NET Framework 版本4.5 的 **一部分。專案** 需要一個參考：

| 參考 dll | 所在位置 | 必須用於 |
|--|--|--|
| Microsoft.IdentityServer.Web.dll | Dll 位於已安裝 AD FS 的 Windows Server 2012 R2 伺服器上的% windir% ADFS 中。<p>此 dll 必須複製到開發電腦，以及在專案中建立的明確參考。 | 介面類別型，包括 IAuthenticationCoNtext、IProofData |

## <a name="create-the-provider"></a>建立提供者

1. 在 Visual Studio 2012：選擇 [檔案->新 >專案 .。。

2. 選取 [類別庫]，並確定您的目標是 .NET 4.5。

    ![[新增專案] 對話方塊的螢幕擷取畫面，其中顯示已選取的 [類別庫] 選項。](media/ad-fs-build-custom-auth-method/Dn783423.71a57ae1-d53d-462b-a846-5b3c02c7d3f2(MSDN.10).jpg "建立提供者")

3. 從已安裝 AD FS 的 Windows Server 2012 R2 伺服器上的% windir% ADFS 複製 **Microsoft.IdentityServer.Web.dll** ，並將其貼入開發電腦上的專案資料夾。

4. 在 **方案總管** 中，以滑鼠右鍵按一下 [ **參考** ]，然後 **新增參考 ...**

5. 流覽至您 **Microsoft.IdentityServer.Web.dll** 的本機複本，然後 **新增 ...**

6. 按一下 **[確定]** 以確認新的參考：

    ![[參考管理員] 對話方塊的螢幕擷取畫面，其中顯示選取的 Microsoft.IdentityServer.Web.dll。](media/ad-fs-build-custom-auth-method/Dn783423.f18df353-9259-4744-b4b6-dd780ce90951(MSDN.10).jpg "建立提供者")

    您現在應該設定來解析提供者所需的所有類型。

7. 將新類別新增至您的專案 (以滑鼠右鍵按一下您的專案，然後 **加入 .。。類別 ...**) ，並為其命名，如下所示 **MyAdapter**：

    ![[加入新專案] 對話方塊的螢幕擷取畫面，其中已選取 [類別] 選項。](media/ad-fs-build-custom-auth-method/Dn783423.6b6a7a8b-9d66-40c7-8a86-a2e3b9e14d09(MSDN.10).jpg "建立提供者")

8. 在 [新增檔案 MyAdapter.cs] 中，以下列程式碼取代現有的程式碼：

    ```csharp
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
    ```

9. 尚未準備好建立 .。。有兩個以上的介面可供前往。

    將兩個類別新增至您的專案：一個適用于中繼資料，另一個用於展示形式。  您可以在與上述類別相同的檔案中新增這些檔案。

    ```csharp
    class MyMetadata : IAuthenticationAdapterMetadata
    {

    }

    class MyPresentationForm : IAdapterPresentationForm
    {

    }
    ```         

10. 接下來，您可以為每個新增必要的成員。首先，中繼資料 (具有有用的內嵌批註) 

    ```csharp
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
    ```

    現在您應該可以 F12 (按一下滑鼠右鍵–移至 IAuthenticationAdapter 上的定義) ，以查看所需的介面成員集合。

    接下來，您可以執行這些動作的簡單執行。

11. 以下列內容取代您類別的整個內容：

    ```csharp
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
    ```

12. 尚未準備好建立 .。。有兩個以上的介面可供前往。

    將兩個類別新增至您的專案：一個適用于中繼資料，另一個用於展示形式。 您可以在與上述類別相同的檔案中新增這些檔案。

    ```csharp
    class MyMetadata : IAuthenticationAdapterMetadata
    {
    }
    class MyPresentationForm : IAdapterPresentationForm
    {
    }
    ```

13. 接下來，您可以為每個新增必要的成員。首先，中繼資料 (具有有用的內嵌批註) 

    ```csharp
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
        /// to determine the best languagelocale to display to the user.
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
        /// "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"
        /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
        /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"
        /// "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"
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
    ```

    接下來是展示形式：

    ```csharp
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
    }
    ```

14. 請注意上述 **FormPageHtml** 元素的 ' todo '。 您可以在一分鐘內修正此問題，但首先讓我們根據新實作為的型別，將最後的必要 return 語句新增至您的初始 MyAdapter 類別。 若要這樣做，請將下列內容新增至您現有的 IAuthenticationAdapter 執行：

    ```csharp
    class MyAdapter : IAuthenticationAdapter
    {
        public IAuthenticationAdapterMetadata Metadata
        {
            //get { return new <instance of IAuthenticationAdapterMetadata derived class>; }
            get { return new MyMetadata(); }
        }

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
    ```

15. 現在適用于包含 html 片段的資源檔。 在您的專案資料夾中，使用下列內容建立新的文字檔：

    ```html
    <div id="loginArea">
        <form method="post" id="loginForm" >
            <!-- These inputs are required by the presentation framework. Do not modify or remove -->
            <input id="authMethod" type="hidden" name="AuthMethod" value="%AuthMethod%" />
            <input id="context" type="hidden" name="Context" value="%Context%" />
            <!-- End inputs are required by the presentation framework. -->
            <p id="pageIntroductionText">This content is provided by the MFA sample adapter. Challenge inputs should be presented below.</p>
            <label for="challengeQuestionInput" class="block">Question text</label>
            <input id="challengeQuestionInput" name="ChallengeQuestionAnswer" type="text" value="" class="text" placeholder="Answer placeholder" />
            <div id="submissionArea" class="submitMargin">
                <input id="submitButton" type="submit" name="Submit" value="Submit" onclick="return AuthPage.submitAnswer()"/>
            </div>
        </form>
        <div id="intro" class="groupMargin">
            <p id="supportEmail">Support information</p>
        </div>
        <script type="text/javascript" language="JavaScript">
            //<![CDATA[
            function AuthPage() { }
            AuthPage.submitAnswer = function () { return true; };
            //]]>
        </script>
    </div>
    ```

16. 然後，選取 [ **專案- \> 新增元件]。資源** 檔並命名檔案 **資源**，然後按一下 [ **新增]：**

    ![[加入新專案] 對話方塊的螢幕擷取畫面，其中顯示已選取的資源檔。](media/ad-fs-build-custom-auth-method/Dn783423.3369ad8f-f65f-4f36-a6d5-6a3edbc1911a(MSDN.10).jpg "建立提供者")

17. 然後，在 **Resources .resx** 檔中，選擇 [ **加入資源]。加入現有** 的檔案。 流覽至包含您在上面儲存的 html 片段)  (文字檔。

    確定您的 GetFormHtml 程式碼會正確地解析資源檔 ( .resx 檔中的新資源名稱，) 名稱前置詞後面接著資源本身的名稱：

    ```csharp
    public string GetFormHtml(int lcid)
    {
        string htmlTemplate = Resources.MfaFormHtml; //Resxfilename.resourcename
        return htmlTemplate;
    }
    ```

   您現在應該能夠建立。

## <a name="build-the-adapter"></a>建立介面卡

介面卡應該內建在強式名稱的 .NET 元件中，該元件可以安裝在 Windows 的 GAC 中。 若要在 Visual Studio 專案中達成此目的，請完成下列步驟：

1. 在方案總管中，以滑鼠右鍵按一下您的專案名稱，然後按一下 [ **屬性**]。

2. 在 [**簽署**] 索引標籤上，核取 [**簽署元件**]，然後選擇 **[選擇強式名稱金鑰** 檔] 下的 [ **<新增 ... >** ：輸入金鑰檔名稱和密碼，然後按一下 **[確定]**。 然後，請確定已核取 **[簽署元件** ]，並取消核取 [ **延遲簽署** ]。 [屬性 **簽署** ] 頁面看起來應該像這樣：

    ![建置您的提供者](media/ad-fs-build-custom-auth-method/Dn783423.0b1a1db2-d64e-4bb8-8c01-ef34296a2668(MSDN.10).jpg "建置您的提供者")

3. 然後建立解決方案。

## <a name="deploy-the-adapter-to-your-ad-fs-test-machine"></a>將介面卡部署到您的 AD FS 測試電腦

在 AD FS 可叫用外部提供者之前，必須先在系統中註冊該提供者。 介面卡提供者必須提供執行必要安裝動作（包括安裝在 GAC 中）的安裝程式，而且安裝程式必須支援 AD FS 中的註冊。 如果沒有這麼做，系統管理員必須執行下列 Windows PowerShell 步驟。 這些步驟可在實驗室中用來啟用測試和偵測。

### <a name="prepare-the-test-ad-fs-machine"></a>準備測試 AD FS 機

複製檔案並新增至 GAC。

1. 確定您有 Windows Server 2012 R2 電腦或虛擬機器。

2. 安裝 AD FS 角色服務，並設定至少有一個節點的伺服器陣列。

    如需在實驗室環境中設定同盟伺服器的詳細步驟，請參閱《 [Windows server 2012 R2 AD FS 部署指南》](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))。

3. 將 Gacutil.exe 工具複製到伺服器。

    Gacutil.exe 可以在 Windows 8 電腦上的 **% homedrive% Program Files (x86) Microsoft sdkswindowsv 8.0 abinnetfx 4.0 工具** 中找到。 您將需要 **gacutil.exe** 檔案本身，以及 **1033**、 **en-us** 和其他當地語系化資源資料夾（位於 **NETFX 4.0 Tools** 位置底下）。

4. 將您的提供者檔案複製 (s)  (一或多個強式名稱簽署的 .dll 檔案) 到與 **gacutil.exe** 相同的資料夾位置， (位置只是為了方便起見) 

5. 將您的 .dll 檔案 (s) 到伺服器陣列中每部 AD FS 同盟伺服器上的 GAC：

    範例：使用命令列工具 GACutil.exe 將 dll 加入至 GAC： `C:>.gacutil.exe /if .<yourdllname>.dll`

    若要在 GAC 中查看產生的專案：`C:>.gacutil.exe /l <yourassemblyname>`


### <a name="register-your-provider-in-ad-fs"></a>在 AD FS 中註冊您的提供者

一旦符合上述必要條件，請在您的同盟伺服器上開啟 Windows PowerShell 命令視窗，然後輸入下列命令 (請注意，如果您使用的是使用 Windows 內部資料庫的同盟伺服器陣列，則必須在伺服器陣列的主要同盟伺服器上執行下列命令) ：

1. `Register-AdfsAuthenticationProvider –TypeName YourTypeName –Name “AnyNameYouWish” [–ConfigurationFilePath (optional)]`

    其中 YourTypeName 是您的 .NET 強型別名稱： "YourDefaultNamespace. YourIAuthenticationAdapterImplementationClassName、YourAssemblyName、Version = YourAssemblyVersion、Culture = 中立、PublicKeyToken = YourPublicKeyTokenValue、processorArchitecture = MSIL"

    這會將您的外部提供者註冊 AD FS 中，並以上述 AnyNameYouWish 所提供的名稱。

2. 使用 [Windows 服務] 嵌入式管理單元重新開機 AD FS 服務 (，例如) 。

3. 執行下列命令：`Get-AdfsAuthenticationProvider`。

    這會將您的提供者顯示為系統中的其中一個提供者。

    範例：

    ```powershell
    $typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”
    Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter”
    net stop adfssrv
    net start adfssrv
    ```

    如果您已在 AD FS 環境中啟用裝置註冊服務，請同時執行下列 PowerShell 命令： `net start drs`

    若要確認已註冊的提供者，請使用下列 PowerShell 命令： `Get-AdfsAuthenticationProvider` 。

    這會將您的提供者顯示為系統中的其中一個提供者。

### <a name="create-the-ad-fs-authentication-policy-that-invokes-your-adapter"></a>建立可叫用介面卡的 AD FS authentication 原則

#### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>使用 AD FS 管理嵌入式管理單元建立驗證原則

1. 從伺服器管理員 [ **工具** ] 功能表) 開啟 [AD FS 管理] 嵌入式管理單元 (。

2. 按一下 [ **驗證原則**]。

3. 在中央窗格的 [ **Multi-Factor Authentication**] 下，按一下 [**全域設定**] 右邊的 [**編輯**] 連結。

4. 在頁面底部的 [ **選取其他驗證方法** ] 下，核取提供者 AdminName 的方塊。 按一下 [套用]。

5. 例如，若要提供「觸發程式」以使用您的介面卡叫用 MFA，請在 [ **位置** ] 底下檢查 **外部** 網路和 **內部** 網路。 按一下 [確定]。  (若要設定每個信賴憑證者的觸發程式，請參閱下面的「使用 Windows PowerShell 建立驗證原則」。 ) 

6. 使用下列命令來檢查結果：

    第一次使用 `Get-AdfsGlobalAuthenticationPolicy` 。 您應該會看到您的提供者名稱作為其中一個 AdditionalAuthenticationProvider 值。

    然後使用 `Get-AdfsAdditionalAuthenticationRule` 。 您應該會看到設定為 [外部網路] 和 [內部網路] 的規則，其設定為系統管理員 UI 中的原則選取結果。

#### <a name="create-the-authentication-policy-using-windows-powershell"></a>使用 Windows PowerShell 建立驗證原則

1. 首先，在全域原則中啟用提供者：

    ```powershell
    Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider “YourAuthProviderName”`
    ```

    > [!NOTE]
    > 請注意，提供給 AdditionalAuthenticationProvider 參數的值會對應至您在上述 Register-AdfsAuthenticationProvider Cmdlet 中為 "Name" 參數提供的值，以及 Get-AdfsAuthenticationProvider Cmdlet 輸出中的 "Name" 屬性。

    ```powershell
    Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider “MyMFAAdapter”`
    ```

2. 接下來，設定全域或信賴憑證者特定的規則來觸發 MFA：

   範例1：若要建立全域規則以要求外部要求的 MFA：
   
   ```powershell
   Set-AdfsAdditionalAuthenticationRule –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'
   ```
   範例2：建立 MFA 規則，要求對特定信賴憑證者進行外部要求的 MFA。  (請注意，個別提供者無法連線至 Windows Server 2012 R2) 中 AD FS 的個別信賴憑證者。

    ```powershell
    $rp = Get-AdfsRelyingPartyTrust –Name <Relying Party Name>
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'
    ```

### <a name="authenticate-with-mfa-using-your-adapter"></a>使用您的介面卡驗證 MFA

最後，執行下列步驟來測試您的介面卡：

1. 確定 AD FS 全域主要驗證類型已設定為外部網路和內部網路的表單驗證 (如此可讓您更輕鬆地以特定使用者身分進行示範) 

    1. 在 AD FS 的嵌入式管理單元中，于 [**驗證原則**] 下的 [**主要驗證**] 區域中，按一下 [**全域設定**] 旁的 [**編輯**]。

        1. 或直接按一下 **多重要素原則** UI 中的 [**主要**] 索引標籤。

2. 確定 **表單驗** 證是針對外部網路和內部網路驗證方法檢查的唯一選項。 按一下 [確定]。

3. 開啟 IDP 起始的登入 html 網頁 (HTTPs:// \<fsname\> /adfs/ls/idpinitiatedsignon.htm) ，並在您的測試環境中以有效的 AD 使用者登入。

4. 輸入主要驗證的認證。

5. 您應該會看到 [MFA 表單] 頁面出現範例挑戰問題。

    如果您已設定多個介面卡，您會看到 [MFA 選項] 頁面上有您的易記名稱。

    ![具有範例挑戰問題的 [M F A forms] 頁面螢幕擷取畫面。](media/ad-fs-build-custom-auth-method/Dn783423.c98d2712-cbd3-4cb9-ac03-2838b81c4f63(MSDN.10).jpg "使用配接器來驗證")

    ![[M F 選擇] 頁面的螢幕擷取畫面。](media/ad-fs-build-custom-auth-method/Dn783423.fd3aefc0-ef6c-4a8c-a737-4914c78ff2d2(MSDN.10).jpg "使用配接器來驗證")

您現在已可正常執行介面，且您已瞭解模型的運作方式。 您可以嘗試做一個額外的範例，在 BeginAuthentication 中設定中斷點以及 TryEndAuthentication。 請注意，當使用者第一次進入 MFA 表單時，會如何執行 BeginAuthentication，而 TryEndAuthentication 則會在每次提交表單時觸發。

## <a name="update-the-adapter-for-successful-authentication"></a>更新介面卡以成功驗證

但是等候–您的範例介面卡永遠無法成功驗證！  這是因為您的程式碼中沒有任何專案針對 TryEndAuthentication 傳回 null。

完成上述程式之後，您建立了基本的介面卡，並將它新增至 AD FS 伺服器。 您可以取得 MFA 表單頁面，但還無法驗證，因為您還沒有在 TryEndAuthentication 執行中放置正確的邏輯。 現在就讓我們加入。

回想一下您的 TryEndAuthentication 實行：

```csharp
public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
{
    //return new instance of IAdapterPresentationForm derived class
    outgoingClaims = new Claim[0];
    return new MyPresentationForm();
}
```

讓我們更新它，使它不一定會傳回 MyPresentationForm ( # A1。 為此，您可以在您的類別內建立一個簡單的公用程式方法：

```csharp
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
```

然後，更新 TryEndAuthentication，如下所示：

```csharp
public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
{
    outgoingClaims = new Claim[0];
    if (ValidateProofData(proofData, authContext))
    {
        //authn complete - return authn method
        outgoingClaims = new[]
        {
            // Return the required authentication method claim, indicating the particulate authentication method used.
            new Claim( "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", "http://example.com/myauthenticationmethod1" )
        };
        return null;
    }
    else
    {
        //authentication not complete - return new instance of IAdapterPresentationForm derived class
        return new MyPresentationForm();
    }
}
```

現在您必須更新 [測試] 方塊上的介面卡。 您必須先復原 AD FS 原則，然後從 AD FS 取消註冊，然後再重新開機 AD FS，然後從 GAC 中移除 .dll，然後將新的 .dll 加入至 GAC，然後將它註冊到 AD FS、重新開機 AD FS，然後重新設定 AD FS 原則。

## <a name="deploy-and-configure-the-updated-adapter-on-your-test-ad-fs-machine"></a>在測試 AD FS 電腦上部署及設定更新的介面卡

### <a name="clear-ad-fs-policy"></a>清除 AD FS 原則

清除 MFA UI 中的所有 MFA 相關核取方塊（如下所示），然後按一下 [確定]。

![清除原則](media/ad-fs-build-custom-auth-method/Dn783423.c111b4e7-5b05-413c-8b0f-222a0e91ac1f(MSDN.10).jpg "清除原則")

### <a name="unregister-provider-windows-powershell"></a>取消註冊提供者 (Windows PowerShell) 

`PS C:> Unregister-AdfsAuthenticationProvider –Name “YourAuthProviderName”`

範例：`PS C:> Unregister-AdfsAuthenticationProvider –Name “MyMFAAdapter”`

請注意，您為 "Name" 傳遞的值與您提供給 Register-AdfsAuthenticationProvider Cmdlet 的「名稱」值相同。 它也是 AdfsAuthenticationProvider 輸出的 "Name" 屬性。

請注意，取消註冊提供者之前，您必須從 Get-adfsglobalauthenticationpolicy (移除提供者，方法是清除 AD FS 管理嵌入式管理單元中簽入的核取方塊，或使用 Windows PowerShell。 ) 

請注意，此作業之後必須重新開機 AD FS 服務。

### <a name="remove-assembly-from-gac"></a>從 GAC 移除元件

1. 首先，使用下列命令來尋找專案的完整強式名稱：`C:>.gacutil.exe /l <yourAdapterAssemblyName>`

    範例：`C:>.gacutil.exe /l mfaadapter`

2. 然後，使用下列命令將它從 GAC 中移除：`.gacutil /u “<output from the above command>”`

    範例：`C:>.gacutil /u “mfaadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

### <a name="add-the-updated-assembly-to-gac"></a>將更新的元件新增至 GAC

請務必先在本機貼上更新的 .dll。 `C:>.gacutil.exe /if .MFAAdapter.dll`

### <a name="view-assembly-in-the-gac-cmd-line"></a>在 GAC 中查看元件 (cmd 行) 

`C:> .gacutil.exe /l mfaadapter`

### <a name="register-your-provider-in-ad-fs"></a>在 AD FS 中註冊您的提供者

1. `PS C:>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.1, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

2. `PS C:>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter1”`

3. 重新啟動 AD FS 服務。

### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>使用 AD FS 管理嵌入式管理單元建立驗證原則

1. 從伺服器管理員 [ **工具** ] 功能表) 開啟 [AD FS 管理] 嵌入式管理單元 (。

2. 按一下 [ **驗證原則**]。

3. 在 [ **Multi-Factor Authentication**] 下，按一下 [**全域設定**] 右邊的 [**編輯**] 連結。

4. 在 [ **選取其他驗證方法**] 下，核取提供者 AdminName 的方塊。 按一下 [套用]。

5. 例如，若要提供「觸發程式」以使用您的介面卡叫用 MFA，請在 [位置] 底下檢查 **外部** 網路和 **內部** 網路。 按一下 [確定]。

### <a name="authenticate-with-mfa-using-your-adapter"></a>使用您的介面卡驗證 MFA

最後，執行下列步驟來測試您的介面卡：

1. 確定 AD FS 全域主要驗證類型已設定為外部網路和內部網路的 **表單驗** 證 (如此可讓您更輕鬆地以特定使用者) 進行驗證。

    1. 在 [AD FS 管理] 嵌入式管理單元的 [**驗證原則**] 底下，按一下 [**主要驗證**] 區域中的 [**全域設定**] 旁的 [**編輯**]。

        1. 或直接按一下多重要素原則 UI 中的 [ **主要** ] 索引標籤。

2. 確定 **表單驗** 證是針對 **外部** 網路和 **內部** 網路驗證方法檢查的唯一選項。 按一下 [確定]。

3. 開啟 IDP 起始的登入 html 網頁 (HTTPs:// \<fsname\> /adfs/ls/idpinitiatedsignon.htm) ，並在您的測試環境中以有效的 AD 使用者登入。

4. 輸入主要驗證的認證。

5. 您應該會看到 [MFA 表單] 頁面出現範例挑戰文字。

    1. 如果您已設定多個介面卡，您會看到 [MFA 選項] 頁面的易記名稱。

在 [MFA 驗證] 頁面輸入 _adfabric_ 時，您應該會看到成功登入。

![M F 表單頁面的螢幕擷取畫面，其中包含範例挑戰文字。](media/ad-fs-build-custom-auth-method/Dn783423.630d8a91-3bfe-4cba-8acf-03eae21530ee(MSDN.10).jpg "使用配接器來登入")

![M F 成功登入頁面的螢幕擷取畫面。](media/ad-fs-build-custom-auth-method/Dn783423.c340fa73-f70f-4870-b8dd-07900fea4469(MSDN.10).jpg "使用配接器來登入")

## <a name="see-also"></a>另請參閱

#### <a name="other-resources"></a>其他資源

[其他驗證方法](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11)) 
[使用機密應用程式的額外 Multi-Factor Authentication 管理風險](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))
