---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: 設定 AD FS 2016 和 Azure MFA
ms.author: billmath
author: billmath
manager: mtillman
ms.date: 01/28/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0a3a08df3789e607a5f154a4735c153867e6046d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960570"
---
# <a name="configure-azure-mfa-as-authentication-provider-with-ad-fs"></a>使用 AD FS 將 Azure MFA 設定為驗證提供者

如果您的組織已和 Azure AD 建立同盟，即可使用 Azure Multi-Factor Authentication 保護 AD FS 資源，雲端和內部部署均包含在內。 Azure MFA 可讓您排除密碼，並提供更安全的驗證方式。  從 Windows Server 2016 開始，您現在可以將 Azure MFA 設定為主要驗證，或使用它做為額外的驗證提供者。 
  
不同于 Windows Server 2012 R2 中的 AD FS，AD FS 2016 Azure MFA adapter 會直接與 Azure AD 整合，而且不需要內部部署 Azure MFA Server。   Azure MFA 介面卡內建于 Windows Server 2016，而且不需要額外的安裝。


## <a name="registering-users-for-azure-mfa-with-ad-fs"></a>向 AD FS 註冊 Azure MFA 的使用者

AD FS 不支援內嵌 &#34;證明&#34;，或註冊 Azure MFA 安全性驗證資訊，例如電話號碼或行動裝置應用程式。 這表示使用者必須在 [https://account.activedirectory.windowsazure.com/Proofup.aspx](https://account.activedirectory.windowsazure.com/Proofup.aspx) 使用 AZURE MFA 向 AD FS 應用程式進行驗證之前造訪，才能完成校訂。 當尚未校訂 Azure AD 的使用者嘗試在 AD FS 使用 Azure MFA 進行驗證時，將會收到 AD FS 錯誤。  身為 AD FS 系統管理員，您可以自訂此錯誤體驗，改為引導使用者前往 proofup 頁面。  您可以使用 onload.js 自訂來偵測 [AD FS] 頁面內的錯誤訊息字串，並顯示新的訊息以引導使用者造訪 [https://aka.ms/mfasetup](https://aka.ms/mfasetup) ，然後重新嘗試驗證。 如需詳細指引，請參閱本文中的「自訂 AD FS 網頁以引導使用者註冊 MFA 驗證方法」。

>[!NOTE]
> 之前，使用者必須使用 MFA 進行驗證以進行註冊（ [https://account.activedirectory.windowsazure.com/Proofup.aspx](https://account.activedirectory.windowsazure.com/Proofup.aspx) 例如透過快捷方式造訪 [https://aka.ms/mfasetup](https://aka.ms/mfasetup) ）。  現在，尚未註冊 MFA 驗證資訊的 AD FS 使用者，可以透過 [https://aka.ms/mfasetup](https://aka.ms/mfasetup) 僅使用主要驗證（例如 Windows 整合式驗證或透過 AD FS 網頁的使用者名稱和密碼）的快捷方式，存取 Azure AD&#34;s proofup 頁面。  如果使用者未設定任何驗證方法，Azure AD 將會執行內嵌註冊，其中使用者會看到訊息，&#34;您的系統管理員要求您設定此帳戶以進行額外的安全性驗證&#34;，然後使用者可以選擇 &#34;立即設定&#34;。
> 已設定至少一個 MFA 驗證方法的使用者，在造訪 proofup 頁面時，仍會提示您提供 MFA。

## <a name="recommended-deployment-topologies"></a>建議的部署拓撲

### <a name="azure-mfa-as-primary-authentication"></a>作為主要驗證的 Azure MFA

使用 Azure MFA 做為 AD FS 的主要驗證有幾個很好的理由：

 - 避免登入 Azure AD、Office 365 和其他 AD FS 應用程式的密碼
 - 若要保護以密碼為基礎的登入，請在密碼之前要求額外的因素，例如驗證碼

如果您想要使用 Azure MFA 做為 AD FS 中的主要驗證方法來達到這些優點，您可能也想要繼續使用 Azure AD 條件式存取，包括 &#34;真正的 MFA&#34;，方法是提示 AD FS 中的其他因素。

您現在可以將 Azure AD 網域設定設為 [在內部部署執行 MFA] （將 &#34;SupportsMfa&#34; 設定為 [$True]）來執行此動作。  在此設定中，Azure AD 會提示 AD FS，以針對需要的條件式存取案例執行額外的驗證或 &#34;真正的 MFA&#34;。  

如上所述，尚未註冊的任何 AD FS 使用者（已設定的 MFA 驗證資訊）都應該透過自訂的 AD FS 錯誤頁面進行提示，以流覽 [https://aka.ms/mfasetup](https://aka.ms/mfasetup) 以設定驗證資訊，然後重新嘗試 AD FS 登入。  
由於 Azure MFA 視為主要因素，因此在初始設定之後，使用者將需要提供額外的因素來管理或更新 Azure AD 中的驗證資訊，或存取其他需要 MFA 的資源。

>[!NOTE]
> 使用 ADFS 2019 時，您必須修改 Active Directory 宣告提供者信任的錨點宣告類型，並將其從 windowsaccountname 修改為 UPN。 執行下面提供的 PowerShell Cmdlet。 這不會影響 AD FS 伺服器陣列的內部運作。 您可能會注意到，在進行此變更後，有少數使用者可能會重新提示您輸入認證。 再次登入之後，終端使用者將不會看到任何差異。 

```powershell
Set-AdfsClaimsProviderTrust -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -TargetName "Active Directory"
```

### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>Azure MFA 做為 Office 365 的額外驗證

先前，如果您想要讓 Azure MFA 做為 Office 365 或其他信賴憑證者 AD FS 中的其他驗證方法，最佳選項是設定 Azure AD 執行複合 MFA，其中主要驗證是在 AD FS 內部執行，而 MFA 則是由 Azure AD 所觸發。 現在，當 [網域 SupportsMfa] 設定設定為 [$True] 時，您可以使用 Azure MFA 做為 AD FS 中的額外驗證。  

如上所述，尚未註冊的任何 AD FS 使用者（已設定的 MFA 驗證資訊）都應該透過自訂的 AD FS 錯誤頁面進行提示，以流覽 [https://aka.ms/mfasetup](https://aka.ms/mfasetup) 以設定驗證資訊，然後重新嘗試 AD FS 登入。  

## <a name="pre-requisites"></a>必要條件

使用 Azure MFA 以 AD FS 進行驗證時，需要下列必要條件：  
  
- [具有 Azure Active Directory 的 Azure 訂用](https://azure.microsoft.com/pricing/free-trial/)帳戶。  
- [Azure Multi-Factor Authentication](/azure/active-directory/authentication/concept-mfa-howitworks) 


> [!NOTE]
> Azure AD 和 Azure MFA 包含在 Azure AD Premium 和企業行動套件（EMS）中。  如果您有上述任一項，則不需要個別訂閱。

- Windows Server 2016 AD FS 內部部署環境。  
   - 伺服器必須能夠透過埠443與下列 Url 進行通訊。
      - https://adnotifications.windowsazure.com
      - https://login.microsoftonline.com
- 您的內部部署環境會[與 Azure AD 同盟。](/azure/active-directory/hybrid/how-to-connect-install-custom#configuring-federation-with-ad-fs)  
- [適用于 Windows PowerShell 的 windows Azure Active Directory 模組](/powershell/module/azuread/?view=azureadps-2.0)。  
- 全域管理員 Azure AD 實例上的許可權，以使用 Azure AD PowerShell 進行設定。  
- 用於設定 Azure MFA 之 AD FS 伺服器陣列的企業系統管理員認證。

## <a name="configure-the-ad-fs-servers"></a>設定 AD FS 伺服器

若要完成適用于 AD FS 的 Azure MFA 設定，您需要使用所述的步驟來設定每個 AD FS 伺服器。 

>[!NOTE]
>請確定已在伺服器陣列中的**所有**AD FS 伺服器上執行這些步驟。 如果您的伺服器陣列中有多部 AD FS 伺服器，您可以使用 Azure AD PowerShell 從遠端執行必要的設定。  

### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>步驟1：使用 Cmdlet 在每部 AD FS 伺服器上產生 Azure MFA 的憑證 `New-AdfsAzureMfaTenantCertificate`

您需要做的第一件事，就是產生憑證供 Azure MFA 使用。  這可以使用 PowerShell 來完成。  產生的憑證可以在 [本機電腦] 憑證存放區中找到，並以主體名稱標示，其中包含您 Azure AD 目錄的 TenantID。

![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  

請注意，TenantID 是您在 Azure AD 中的目錄名稱。  使用下列 PowerShell Cmdlet 來產生新的憑證。  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  

![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-the-azure-multi-factor-auth-client-service-principal"></a>步驟2：將新認證新增至 Azure 多因素驗證用戶端服務主體

為了讓 AD FS 伺服器能夠與 Azure 多因素驗證用戶端通訊，您必須將認證新增至 Azure 多因素驗證用戶端的服務主體。 使用 Cmdlet 產生的 `New-AdfsAzureMFaTenantCertificate` 憑證將會作為這些認證。 請使用 PowerShell 來執行下列動作，以將新認證新增至 Azure 多因素驗證用戶端服務主體。  

> [!NOTE]
> 若要完成此步驟，您需要使用 PowerShell 連接到您的 Azure AD 實例 `Connect-MsolService` 。  這些步驟假設您已透過 PowerShell 連線。  如需詳細資訊，請參閱[ `Connect-MsolService` 。](/previous-versions/azure/dn194123(v=azure.100))  

**將憑證設定為針對 Azure 多因素驗證用戶端的新認證**  

`New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64`

> [!IMPORTANT]
> 此命令必須在伺服器陣列中的所有 AD FS 伺服器上執行。  在未將憑證設定為 Azure 多因素驗證用戶端的新認證的伺服器上，Azure AD MFA 將會失敗。

> [!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 是 Azure 多因素驗證用戶端的 GUID。  
  
## <a name="configure-the-ad-fs-farm"></a>設定 AD FS 伺服器陣列  
  
當您完成每個 AD FS 伺服器上的上一節之後，請使用[AdfsAzureMfaTenant](/powershell/module/adfs/export-adfsauthenticationproviderconfigurationdata) Cmdlet 來設定 Azure 租使用者資訊。 此 Cmdlet 只需要針對 AD FS 伺服器陣列執行一次。

開啟 PowerShell 提示字元，並使用[AdfsAzureMfaTenant](/powershell/module/adfs/export-adfsauthenticationproviderconfigurationdata) Cmdlet 輸入您自己的*tenantId* 。 針對使用 Microsoft Azure Government cloud 的客戶，請新增 `-Environment USGov` 參數：

> [!NOTE]
> 您必須先重新開機伺服器陣列中每部伺服器上的 AD FS 服務，這些變更才會生效。 針對最小的影響，請將每個 AD FS 伺服器一次從 NLB 輪替中執行，並等候所有連線清空。

```powershell
Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720
```

![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)

沒有最新 Service Pack 的 Windows Server 不支援 `-Environment` [AdfsAzureMfaTenant](/powershell/module/adfs/export-adfsauthenticationproviderconfigurationdata) Cmdlet 的參數。 如果您使用 Azure Government 雲端，而先前的步驟因遺失參數而無法設定 Azure 租使用者 `-Environment` ，請完成下列步驟以手動建立登錄專案。 如果先前的 Cmdlet 正確地註冊您的租使用者資訊，或您不在 Azure Government 雲端中，請略過這些步驟：

1. 在 AD FS 伺服器上開啟 [**登錄編輯程式**]。
1. 瀏覽至 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ADFS`。 建立下列登錄機碼值：

    | 登錄機碼       | 值 |
    |--------------------|-----------------------------------|
    | SasUrl             | https://adnotifications.windowsazure.us/StrongAuthenticationService.svc/Connector |
    | StsUrl             | https://login.microsoftonline.us |
    | ResourceUri        | https://adnotifications.windowsazure.us/StrongAuthenticationService.svc/Connector |

1. 請先重新開機伺服器陣列中每部伺服器上的 AD FS 服務，這些變更才會生效。 針對最小的影響，請將每個 AD FS 伺服器一次從 NLB 輪替中執行，並等候所有連線清空。

在此之後，您會看到 Azure MFA 可作為內部網路和外部網路使用的主要驗證方法。

![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>更新和管理 AD FS Azure MFA 憑證

下列指導方針會引導您瞭解如何管理 AD FS 伺服器上的 Azure MFA 憑證。
根據預設，當您使用 Azure MFA 設定 AD FS 時，透過 PowerShell Cmdlet 產生的憑證 `New-AdfsAzureMfaTenantCertificate` 有效期限為2年。  若要判斷憑證的到期日，然後更新並安裝新的憑證，請使用下列程式。

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>評估 AD FS Azure MFA 憑證到期日

在每部 AD FS 伺服器上，在本機電腦的 [我的存放區] 中，將會有一個具有 &#34;OU = Microsoft AD FS Azure MFA&#34; 的自我簽署憑證（在簽發者和主體中）。  這是 Azure MFA 憑證。  請檢查每個 AD FS 伺服器上此憑證的有效期間，以判斷到期日。  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>在每部 AD FS 伺服器上建立新的 AD FS Azure MFA 憑證

如果您憑證的有效期間即將結束，請在每部 AD FS 伺服器上產生新的 Azure MFA 憑證，以開始更新程式。 在 PowerShell 命令視窗中，使用下列 Cmdlet 在每部 AD FS 伺服器上產生新的憑證：

> [!CAUTION]
> 如果您的憑證已過期，請不要將 `-Renew $true` 參數新增至下列命令。 在此案例中，現有過期的憑證會取代為新的憑證，而不會保留在原處，而是建立額外的憑證。

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

如果憑證尚未過期，則會產生一個從未來2天起算的新憑證到2天 + 2 年。 AD FS 和 Azure MFA 作業不會受到此 Cmdlet 或新憑證的影響。 （請注意：這兩天的延遲是刻意的，並提供時間來執行下列步驟，在租使用者中設定新憑證，然後 AD FS 開始使用它來進行 Azure MFA）。

### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>在 Azure AD 租使用者中設定每個新的 AD FS Azure MFA 憑證

使用 Azure AD PowerShell 模組，針對每個新憑證（在每部 AD FS 伺服器上）更新您的 Azure AD 租使用者設定，如下所示（注意：您必須先使用連線到租使用者， `Connect-MsolService` 才能執行下列命令）。

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $newcert
```

如果您先前的憑證已過期，請重新開機 AD FS 服務以挑選新的憑證。 如果您在憑證到期之前已更新，則不需要重新開機 AD FS 服務。

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>確認新的憑證將用於 Azure MFA

一旦新的憑證生效後，AD FS 將會在幾個小時內將其挑選並開始使用 Azure MFA 的每個個別憑證。  一旦發生這種情況，您將會在每部伺服器上看到記錄在 AD FS Admin 事件記錄檔中的事件，其中包含下列資訊：

```
Log Name:      AD FS/Admin
Source:        AD FS
Date:          2/27/2018 7:33:31 PM
Event ID:      547
Task Category: None
Level:         Information
Keywords:      AD FS
User:          DOMAIN\adfssvc
Computer:      ADFS.domain.contoso.com
Description:
The tenant certificate for Azure MFA has been renewed.  

TenantId: contoso.onmicrosoft.com.
Old thumbprint: 7CC103D60967318A11D8C51C289EF85214D9FC63.
Old expiration date: 9/15/2019 9:43:17 PM.
New thumbprint: 8110D7415744C9D4D5A4A6309499F7B48B5F3CCF.
New expiration date: 2/27/2020 2:16:07 AM.
```

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>自訂 AD FS 網頁，引導使用者註冊 MFA 驗證方法

使用下列範例，為尚未校訂的使用者（已設定的 MFA 驗證資訊）自訂您的 AD FS 網頁。

### <a name="find-the-error"></a>尋找錯誤

首先，有幾個不同的錯誤訊息 AD FS 會在使用者缺少驗證資訊的情況下傳回。
如果您使用 Azure MFA 做為主要驗證，取消校訂的使用者會看到包含下列訊息的 AD FS 錯誤頁面：
```
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information.
        </div>
```
當嘗試進行其他驗證 Azure AD 時，校訂的使用者會看到包含下列訊息的 AD FS 錯誤頁面：
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

### <a name="catch-the-error-and-update-the-page-text"></a>攔截錯誤並更新頁面文字

若要攔截錯誤並顯示使用者自訂指引，只要將 javascript 附加至屬於 AD FS web 主題的 onload.js 檔案結尾即可。  這可讓您執行下列動作：
 - 搜尋識別錯誤字串
 - 提供自訂 web 內容。  

> [!NOTE]
> 如需有關如何自訂 onload.js 檔案的一般指引，請參閱[AD FS 登入頁面的自訂一](advanced-customization-of-ad-fs-sign-in-pages.md)文。

以下是一個簡單的範例，您可能會想要擴充：

1. 在您的主要 AD FS 伺服器上開啟 Windows PowerShell，然後執行下列命令來建立新的 AD FS Web 主題：
    
    ``` PowerShell
        New-AdfsWebTheme –Name ProofUp –SourceName default
    ``` 
2. 接下來，建立資料夾並匯出預設 AD FS Web 主題：

    ``` PowerShell
       New-Item -Path 'c:\Theme' -ItemType Directory;Export-AdfsWebTheme –Name default –DirectoryPath c:\Theme
    ```
3. 在文字編輯器中開啟 C:\Theme\script\onload.js 檔案
4. 將下列程式碼附加至 onload.js 檔案的結尾
    
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
        if (errorMessage) {
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
    }

    //End Customize MFA Exception
    //End Custom Code
    ```
    > [!IMPORTANT]
    > 您需要變更「<YOUR_DOMAIN_NAME_HERE>」;以使用您的功能變數名稱。 例如：`var domain_hint = "contoso.com";`
    
5. 儲存 onload.js 檔案
6. 輸入下列 Windows PowerShell 命令，將 onload.js 檔案匯入您的自訂主題：
    
    ``` PowerShell
    Set-AdfsWebTheme -TargetName ProofUp -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';path="c:\theme\script\onload.js"}
    ```
7. 最後，輸入下列 Windows PowerShell 命令，以套用自訂 AD FS Web 主題：
    
    ``` PowerShell
    Set-AdfsWebConfig -ActiveThemeName "ProofUp"
    ```

## <a name="next-steps"></a>接下來的步驟

[管理 AD FS 和 Azure MFA 所使用的 TLS/SSL 通訊協定和加密套件](manage-ssl-protocols-in-ad-fs.md)
