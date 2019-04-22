---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: 設定 AD FS 2016 和 Azure MFA
description: ''
ms.author: billmath
author: billmath
manager: mtillman
ms.date: 01/28/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ae7809089a69ac0ff48168db0aa2e9d61c35257a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814089"
---
# <a name="configure-azure-mfa-as-authentication-provider-with-ad-fs"></a>將 Azure MFA 設定 AD FS 驗證提供者

>適用於：Windows Server 2016、windows Server 2019

如果您的組織與 Azure AD 同盟，您可以使用 Azure Multi-factor Authentication 來保護 AD FS 資源，同時對內部部署和雲端中。 Azure MFA 可讓您消除密碼，並提供更安全的方式來進行驗證。  您現在可以從 Windows Server 2016 開始，設定 Azure MFA 進行主要驗證或使用它作為其他驗證提供者。 
  
不同於與 Windows Server 2012 r2 中的 AD FS 的 AD FS 2016 Azure MFA 配接器直接與 Azure AD 整合且不需要在內部部署 Azure MFA server。   Azure MFA 配接器內建 Windows Server 2016 中，並不需要額外的安裝。


## <a name="registering-users-for-azure-mfa-with-ad-fs"></a>與 AD FS 的 Azure mfa 中註冊使用者

AD FS 不支援內嵌&#34;註冊的證明&#34;，或註冊 Azure MFA 的安全性驗證資訊，例如電話號碼或行動裝置應用程式。 這表示使用者必須取得已經過證明的設定，請造訪[ https://account.activedirectory.windowsazure.com/Proofup.aspx ](https://account.activedirectory.windowsazure.com/Proofup.aspx)之前使用 Azure MFA 來驗證 AD FS 的應用程式。 當有不尚未已經過證明的註冊在 Azure AD 會嘗試使用在 AD FS 的 Azure MFA 驗證的使用者時，他們會收到 AD FS 錯誤。  做為 AD FS 系統管理員，您可以自訂這個錯誤體驗，以改為 proofup 頁面來引導使用者。  您可以執行這項操作來偵測 AD FS 網頁內的錯誤訊息字串，並顯示新的訊息，引導使用者前往的網站使用 onload.js 自訂[ https://aka.ms/mfasetup ](https://aka.ms/mfasetup)，然後重新嘗試驗證。 詳細的指導方針請參閱 「 自訂 AD FS 網頁來引導使用者來註冊 MFA 驗證方法 」 下, 面這篇文章中。

>[!NOTE]
> 先前，使用者需要使用 MFA 進行註冊驗證 (瀏覽[ https://account.activedirectory.windowsazure.com/Proofup.aspx ](https://account.activedirectory.windowsazure.com/Proofup.aspx)，例如透過快顯[ https://aka.ms/mfasetup ](https://aka.ms/mfasetup))。  現在，尚未註冊 MFA 驗證資訊的 AD FS 使用者可以存取 Azure AD&#34;s proofup 頁面，透過快顯[ https://aka.ms/mfasetup ](https://aka.ms/mfasetup)僅使用主要驗證 (例如 Windows 整合式驗證或使用者名稱和密碼透過 AD FS 的網頁）。  如果使用者不沒有設定任何驗證方法，Azure AD 會執行使用者會看到此訊息的內嵌註冊&#34;系統管理員已要求您設定此帳戶的其他安全性驗證&#34;，而使用者接著可以若要選取&#34;立即設定&#34;。
> 已設定至少一個 MFA 驗證方法的使用者仍會提示您瀏覽 proofup 頁面時，提供 MFA。

## <a name="recommended-deployment-topologies"></a>建議的部署拓撲

### <a name="azure-mfa-as-primary-authentication"></a>做為主要驗證的 azure MFA

有幾個很棒的原因，做為主要驗證與 AD FS 使用 Azure MFA:

 - 若要避免密碼進行登入 Azure AD、 Office 365 和其他 AD FS 應用程式
 - 若要保護的密碼型登入要求的其他因素，例如密碼之前的驗證碼

如果您想要做為主要的驗證方法，在 AD FS 使用 Azure MFA，來達成這些權益，您可能也想来保留能夠使用 Azure AD 條件式存取，包括&#34;true MFA&#34;會提示在 AD FS 中的其他因素。

現在您可以藉由設定要在內部部署 MFA 的 Azure AD 網域設定 (設定&#34;SupportsMfa&#34;以 $True)。  在此組態中，AD FS 可以會提示您 Azure ad 執行額外的驗證或&#34;true MFA&#34;需要此參數的條件式存取案例。  

如上面所述，任何 AD FS 使用者尚未註冊 （已設定的 MFA 驗證資訊） 應該會提示使用者透過自訂的 AD FS 錯誤頁面瀏覽[ https://aka.ms/mfasetup ](https://aka.ms/mfasetup)設定驗證資訊，然後重新嘗試 AD FS 登入。  
因為初始設定使用者必須提供其他的因素，來管理或更新其驗證資訊，在 Azure AD 中，或存取需要 MFA 的其他資源之後，設為主要的 Azure MFA 會被視為單一的因數，。

>[!NOTE]
> 使用 ADFS 2019，您必須請修改 Active Directory 宣告提供者信任的錨點宣告類型和修改這個從 windowsaccountname 與 UPN。 執行下面提供的 powershell commandlet 如下。 並不會影響在 AD FS 伺服器陣列的內部運作方式。 您可能會發現一些使用者可能 reprompted 認證，這項變更之後。 之後再次登入，使用者會看到任何差異。 

```powershell
Set-AdfsClaimsProviderTrust -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -TargetName "Active Directory"
```

### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>作為其他驗證 Office 365 的 azure MFA

先前，如果您想要有 Azure MFA AD FS 中其他驗證方法，為 Office 365 或其他信賴憑證者的合作對象，最好的選擇是要設定 Azure AD，以執行複合的 MFA，在 MFA 和 AD FS 在內部部署執行主要驗證之是 trAzure AD 所 iggered。 現在，您可以使用 Azure MFA 作為 AD FS 中的其他驗證網域 SupportsMfa 設定設為 $True 時。  

如上面所述，任何 AD FS 使用者尚未註冊 （已設定的 MFA 驗證資訊） 應該會提示使用者透過自訂的 AD FS 錯誤頁面瀏覽[ https://aka.ms/mfasetup ](https://aka.ms/mfasetup)設定驗證資訊，然後重新嘗試 AD FS 登入。  

## <a name="pre-requisites"></a>必要條件

使用 Azure MFA 與 AD FS 驗證時，所需下列必要條件：  
  
- [與 Azure Active Directory 的 Azure 訂用帳戶](https://azure.microsoft.com/pricing/free-trial/)。  
- [Azure Multi-factor Authentication](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/)  
- 透過連接埠 80 和 443 是能夠以下列 communticate web 應用程式 proxy

    - https://adnotifications.windowsazure.com
    - https://login.microsoftonline.com


> [!NOTE]
> Azure AD 和 Azure AD Premium 和 Enterprise Mobility Suite (EMS) 中包含 Azure MFA。  如果您有任一種您不需要個別訂用帳戶。
- Windows Server 2016 AD FS 在內部部署環境。  
   - 伺服器必須能夠透過連接埠 80 和 443 與下列 Url 通訊。
      - https://adnotifications.windowsazure.com
      - https://login.microsoftonline.com
- 您的內部部署環境是[與 Azure AD 同盟。](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [適用於 Windows PowerShell 的 Windows Azure Active Directory 模組](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0)。  
- 全域系統管理員權限的 Azure AD 執行個體上使用 Azure AD PowerShell 加以設定。  
- 若要設定 Azure MFA AD FS 伺服器陣列的企業系統管理員認證。  
  
## <a name="configure-the-ad-fs-servers"></a>設定 AD FS 伺服器

若要完成適用於 AD FS 設定 Azure MFA，您需要設定每個 AD FS 伺服器，使用所述的步驟。 

>[!NOTE]
>確保在執行這些步驟**所有**伺服陣列中的 AD FS 伺服器。 如果您有多個 AD FS 伺服器陣列中時，您可以執行必要的設定，從遠端使用 Azure AD Powershell。  

### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>步驟 1：為 Azure MFA 中產生的憑證，在每個 AD FS 伺服器使用`New-AdfsAzureMfaTenantCertificate`cmdlet

您只需要第一件事是產生要使用的 Azure mfa 憑證。  這可以使用 PowerShell。  產生的憑證可在本機電腦憑證存放區，並標示包含您的 Azure AD 目錄的 TenantID，主體名稱。

![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  

請注意，TenantID 您目錄的名稱在 Azure AD 中。  您可以使用下列 PowerShell cmdlet 來產生新的憑證。  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  

![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-the-azure-multi-factor-auth-client-service-principal"></a>步驟 2：將新的認證新增至 Azure 多重要素驗證用戶端服務主體

若要讓 AD FS 伺服器與 Azure 多重要素驗證用戶端進行通訊，您需要將認證新增至服務主體的 Azure 多重要素驗證用戶端。 使用產生的憑證`New-AdfsAzureMFaTenantCertificate`cmdlet 將做為這些認證。 執行下列動作來新增新的認證給 Azure 多重要素驗證用戶端服務主體中使用 PowerShell。  

> [!NOTE]
> 為了完成此步驟中，您必須連接到您的 Azure AD PowerShell 中使用 Connect-msolservice 和執行個體。  這些步驟假設您已透過 PowerShell 連線。  如需資訊[Connect-msolservice。](https://msdn.microsoft.com/library/dn194123.aspx)  

**將憑證設定為新的認證，對 Azure 多重要素驗證用戶端**  

`New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64`

> [!IMPORTANT]
> 此命令必須在所有伺服器陣列中的 AD FS 伺服器上執行。  Azure AD MFA，將會失敗有沒有設定為新的認證，對 Azure 多重要素驗證用戶端憑證的伺服器上。

> [!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 是 Azure 多重要素驗證用戶端的 GUID。  
  
## <a name="configure-the-ad-fs-farm"></a>設定 AD FS 伺服器陣列  
  
一旦您完成每個 AD FS 伺服器上的一節中，您必須執行`Set-AdfsAzureMfaTenant`cmdlet。  
  
此 cmdlet 需要 AD FS 伺服器陣列執行一次。  使用 PowerShell 來完成此步驟。
  
> [!NOTE]  
> 您必須在伺服陣列中重新啟動每部伺服器上的 AD FS 服務，這些變更才會影響。  
  
    Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720  

![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)  
  
在此之後，您會看到 Azure MFA 可做為內部網路和外部網路使用主要驗證方法。    
  
![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>更新和管理 AD FS 的 Azure MFA 憑證

下列指引會引導您如何管理您的 AD FS 伺服器上的 Azure MFA 憑證。
根據預設，當您設定 AD FS 與 Azure MFA，透過新增 AdfsAzureMfaTenantCertificate PowerShell cmdlet 產生的憑證有效期為 2 年。  若要判斷如何關閉 以到期憑證，以及之後更新，並安裝新的憑證，請使用下列程序。

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>評估 AD FS 的 Azure MFA 憑證到期日

每個 AD FS 伺服器，在本機電腦上我的存放區中，會使用自我簽署的憑證&#34;OU = Microsoft AD FS 的 Azure MFA&#34;中的簽發者和主旨。  這是 Azure MFA 憑證。  請檢查此憑證來決定到期日的每個 AD FS 伺服器上的有效期間。  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>每個 AD FS 伺服器上建立新的 AD FS Azure MFA 憑證

如果您的憑證有效期間即將結束，請藉由產生新的 Azure MFA 憑證，每個 AD FS 伺服器上啟動更新程序。 在 powershell 命令視窗中，會產生新的憑證，使用下列 cmdlet 的每部 AD FS 伺服器上：

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

由於此 cmdlet，將會產生新憑證的有效 2 天在未來的 2 天 + 2 年。  AD FS 與 Azure MFA 的作業將不會受到這個指令程式或新的憑證。 (注意： 2 天的延遲是刻意設計的並提供執行下列步驟來設定租用戶中的新憑證，AD FS 可讓您開始使用 Azure mfa 之前的時間。)

### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>在 Azure AD 租用戶中設定每個新的 AD FS 的 Azure MFA 憑證

使用 Azure AD PowerShell 模組中，為每個新的憑證 （在每個 AD FS 伺服器），更新您的 Azure AD 租用戶設定，如下所示 (請注意： 您必須先連接到租用戶中執行下列命令中使用 Connect-msolservice)。

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $newcert
```

$certbase64 是新的憑證。  您可以透過匯出憑證 （不含私密金鑰），為 DER 編碼的檔案和開啟 notepad.exe，然後複製/貼上至 PSH 工作階段與指派給變數 $certbase64 取得 base64 編碼的憑證

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>請確認新的憑證將用於 Azure MFA

一旦新的憑證生效，AD FS 會加以提取，並開始使用 Azure MFA 以一天的幾小時內每個個別的憑證。  一旦發生這種情況，在每一部伺服器上您會看到事件記錄在 AD FS 系統管理員事件記錄檔，使用下列資訊：記錄檔名稱：    AD FS/系統管理員來源：      AD FS 日期：        2018 年 2 月 27 日下午 7:33:31 事件識別碼：    547 工作類別：沒有任何層級：       資訊的關鍵字：    AD FS 使用者：        DOMAIN\adfssvc 電腦：    ADFS.domain.contoso.com 描述：已更新 Azure MFA 的租用戶憑證。  

TenantId: contoso.onmicrosoft.com。
舊的憑證指紋：7CC103D60967318A11D8C51C289EF85214D9FC63.
舊的到期日：9/15/2019年下午 9:43:17。
新的憑證指紋：8110D7415744C9D4D5A4A6309499F7B48B5F3CCF.
新的到期日：2020 年 2/27/2:16:07 AM。

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>自訂 AD FS 網頁，以引導使用者註冊 MFA 驗證方法

使用下列範例以自訂您的使用者有不尚未已經過證明的設定 AD FS 的網頁 （設定 MFA 驗證資訊）。

### <a name="find-the-error"></a>找出錯誤

首先，有幾個 AD FS 會在使用者在其中缺少驗證資訊的情況下傳回不同的錯誤訊息。
如果您使用 Azure MFA 作為主要驗證，則未校訂的使用者會看到 ADFS 錯誤頁面包含下列訊息：
```
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information.
        </div>
```
當嘗試為其他驗證 Azure AD 時，未校訂使用者會看到 ADFS 錯誤頁面包含下列訊息：
```
<div id='mfaGreetingDescription' class='groupMargin'>For security reasons, we require additional information to verify your account (mahesh@jenfield.net)</div>
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            The selected authentication method is not available for &#39;username@contoso.com&#39;. Choose another authentication method or contact your system administrator for details.
        </div>
```

### <a name="catch-the-error-and-update-the-page-text"></a>攔截到錯誤，並更新頁面文字

若要攔截到錯誤，並向使用者顯示自訂指導方針只是附加 javascript onload.js 檔案屬於 AD FS 的網頁佈景主題的結尾。  這可讓您執行下列作業：
 - 識別錯誤字串搜尋
 - 提供自訂的 web 內容。  

(如需如何自訂 onload.js 檔案的一般指引，請參閱文章[Advanced Customization of AD FS sign-in Pages](advanced-customization-of-ad-fs-sign-in-pages.md)。)

以下是簡單的範例，您可能想要擴充：

1. 主要 AD FS 伺服器上開啟 Windows PowerShell 並執行下列命令來建立新的 AD FS 網頁佈景主題：
    
    ``` PowerShell
        New-AdfsWebTheme –Name ProofUp –SourceName default
    ``` 
2. 接下來，匯出預設 AD FS 網頁佈景主題：

    ``` PowerShell
       Export-AdfsWebTheme –Name default –DirectoryPath c:\Theme
    ```
3. 在 文字編輯器中開啟 C:\Theme\script\onload.js 檔案
4. 將下列程式碼附加至 onload.js 檔案結尾
    
    ``` JavaScript
    //Custom Code
    //Customize MFA exception
    //Begin

    var domain_hint = "<YOUR_DOMAIN_NAME_HERE>";
    var mfaSecondFactorErr = "The selected authentication method is not available for";
    var mfaProofupMessage = "You will be automatically redirected in 5 seconds to set up your account for additional security verification. Once you have completed the setup, please return to the application you are attempting to access.<br><br>If you are not redirected automatically, please click <a href='{0}'>here</a>."
    var authArea = document.getElementById("authArea");
    if (authArea) {
        var errorMessage = document.getElementById("errorMessage");
        if (errorMessage.innerHTML.indexOf(mfaSecondFactorErr) >= 0) {

        //Hide the error message
            var openingMessage = document.getElementById("openingMessage");
            if (openingMessage) {
                openingMessage.style.display = 'none'
            }
            var errorDetailsLink = document.getElementById("errorDetailsLink");
            if (errorDetailsLink) {
                errorDetailsLink.style.display = 'none'
            }

            //Provide a message and redirect to Azure AD MFA Registration Url
            var mfaRegisterUrl = "https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1&whr=" + domain_hint;
            errorMessage.innerHTML = "<br>" + mfaProofupMessage.replace("{0}", mfaRegisterUrl);
            window.setTimeout(function () { window.location.href = mfaRegisterUrl; }, 5000);
        }
    }

    //End Customize MFA Exception
    //End Custom Code
    ```
    > [!IMPORTANT]
    > 您需要變更 「 < YOUR_DOMAIN_NAME_HERE >";若要使用您的網域名稱。 例如：`var domain_hint = "contoso.com";`
    
5. 儲存 onload.js 檔案
6. 將 onload.js 檔案匯入您的自訂佈景主題中輸入下列 Windows PowerShell 命令：
    
    ``` PowerShell
    Set-AdfsWebTheme -TargetName ProofUp -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}
    ```
7. 最後，將自訂的 AD FS 網頁佈景主題套用輸入下列 Windows PowerShell 命令：
    
    ``` PowerShell
    Set-AdfsWebConfig -ActiveThemeName
    ```

## <a name="next-steps"></a>後續步驟

[管理 TLS/SSL 通訊協定與 Azure MFA 和 AD FS 所使用的加密套件](manage-ssl-protocols-in-ad-fs.md)
