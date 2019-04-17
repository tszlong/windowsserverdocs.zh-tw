---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: "AD FS 2016 與 Azure MFA 設定"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/01/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be8e88ae36f344f1265fb76e66c19e0ac8aeb533
ms.sourcegitcommit: ffdfbb76c7f775e20d089d3f662d4f9a7d642f1e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2018
---
# <a name="configure-ad-fs-2016-and-azure-mfa"></a>AD FS 2016 與 Azure MFA 設定

>適用於：Windows Server 2016

如果您的組織會聯盟使用 Azure AD，您可以使用 Azure 多因素驗證安全 AD FS 資源，這兩個上場所和雲端。 Azure MFA 可讓您能排除的密碼，並提供更安全的驗證方式。  開始使用 Windows Server 2016，您現在可以設定 Azure MFA 主要驗證。 
  
不同於在 Windows Server 2012 R2，AD FS 使用 AD FS 2016 Azure MFA 介面卡直接整合 Azure AD 並不需要在場所 Azure MFA 伺服器。   Azure MFA 介面卡是 Windows Server 2016 並不需要其他安裝的。

## <a name="registering-users-for-azure-mfa-with-ad-fs-2016"></a>登記使用者的使用 AD FS 2016 Azure MFA
AD FS 不情形」證明向上」或登記 Azure MFA 安全性驗證資訊，例如電話號碼或行動裝置版」app 的支援。 這表示使用者必須取得 proofed 瀏覽 https://account.activedirectory.windowsazure.com/Proofup.aspx 之前使用 Azure MFA 驗證，AD FS 應用程式。 當有不尚未 proofed 向上 Azure AD 嘗試驗證，AD FS Azure MFA 使用中的使用者時，他們將會取得 AD FS 錯誤。  AD FS 管理員的身分、為您可以自訂使用者指南 proofup 頁面改為使用此錯誤經驗。  您可以執行此動作偵測 AD FS 頁面中的錯誤訊息字串，並顯示新訊息引導 https://aka.ms/mfasetup，瀏覽的使用者使用 onload.js 自訂項目，然後再重新嘗試驗證。 詳細指導方針看到「自訂 AD FS 網頁引導使用者登記 MFA 驗證方法「下方這篇文章中。

>[!NOTE]
> 之前，需要使用者 MFA 與驗證的登記（造訪 https://account.activedirectory.windowsazure.com/Proofup.aspx，例如透過快顯 aka.ms/mfasetup）。  現在，已經不尚未登記 MFA 驗證資訊 AD FS 人可以存取 Azure AD 的 proofup 網頁快速鍵 aka.ms/mfasetup 透過使用只主要驗證（例如整合式的 Windows 驗證或使用者名稱和密碼 AD FS 透過網頁）.  如果使用者不有任何設定的驗證方法、Azure AD 會執行情形登記使用者看到的訊息中的 [您的系統管理員已所需設定此負責額外的安全性驗證」，然後選取使用者」設定現在「。
> 已經設定一個以上 MFA 驗證方法使用者將仍然會提示您提供 MFA proofup 網頁瀏覽時。

### <a name="recommended-deployment-topologies"></a>建議的部署拓撲

#### <a name="azure-mfa-as-primary-authentication"></a>Azure MFA 做為主要驗證
有幾個要作為主要驗證，AD FS 使用 Azure MFA 變得更好的原因︰
 - 若要避免使用密碼登入 Azure AD 的 Office 365 和其他 AD FS 應用程式
 - 若要保護密碼依登入需要額外因素例如密碼前驗證碼

如果您想要使用做為主要的驗證方法 AD FS 中 Azure MFA 實現優點，您可能也會想来保留條件使用 Azure AD 的功能包括「true MFA「提示額外因素 AD FS 中存取。

現在您可以設定 Azure AD 網域 MFA 場所（設定「SupportsMfa「$True）上的執行動作。  此設定，AD FS 可將提示 Azure AD 進行額外的驗證或「true MFA「需要的條件存取案例。  

如上文所述，任何 AD FS 的使用者尚未且已（設定的 MFA 驗證資訊）應該會提示您透過自訂 AD FS 錯誤網頁瀏覽 aka.ms/mfasetup 進行驗證的資訊，然後再重新嘗試 AD FS 登入。  
因為 Azure MFA 為主要的單一因數之後將需要提供額外因素管理或更新中 Azure AD 時，他們驗證的資訊或存取其他資源需要 MFA 初始設定的使用者。


#### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>Azure MFA 做為額外的驗證，Office 365
之前，如果您有 MFA Azure AD FS 中 Office 365 或其他信賴的對象，最好額外的驗證方法就是設定 Azure AD 以執行複合的 MFA，AD FS 和 MFA 場所中執行主要驗證是 trAzure AD，iggered。 現在，您可以使用 Azure MFA 做為額外的驗證，AD FS 中網域 SupportsMfa 設定設定為 $True 時。  

如上文所述，任何 AD FS 的使用者尚未且已（設定的 MFA 驗證資訊）應該會提示您透過自訂 AD FS 錯誤網頁瀏覽 aka.ms/mfasetup 進行驗證的資訊，然後再重新嘗試 AD FS 登入。  


## <a name="pre-requisites"></a>必要條件  
使用 Azure MFA AD FS 進行驗證時，所需下列必要條件：  
  
- [使用 Azure Active Directory Azure 裝機費](https://azure.microsoft.com/pricing/free-trial/)。  
- [Azure 多因素驗證](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/)  

>[!NOTE]   
> Azure AD 以及 Azure MFA 包含 Azure AD Premium 和 Enterprise Mobility Suite (EMS)。  如果您擁有的其中您不需要個人月租方案。   
- Windows Server 2016 AD FS 先環境中。  
- 您在場所環境[使用 Azure AD 聯盟。](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [Windows Azure Active Directory Windows PowerShell 模組](https://go.microsoft.com/fwlink/p/?linkid=236297)。  
- 全域系統管理員權限在您的 Azure AD 的執行個體來設定使用 Azure AD PowerShell。  
- 若要設定 MFA Azure AD FS 發電廠企業系統管理員認證。  
  
  
## <a name="configure-the-ad-fs-servers"></a>設定 AD FS 伺服器  
以完成設定 MFA Azure AD fs，您需要每個 AD FS 使用設定伺服器所述。 

>[!NOTE]
>確保，執行下列步驟執行**所有**AD FS 伺服器。 如果您有多個 AD FS 伺服器陣列中，您可以執行從遠端使用 Azure AD Powershell 必要的設定。  
  
  
  
### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>步驟 1：產生 Azure MFA 的每個 AD FS 伺服器使用憑證`New-AdfsAzureMfaTenantCertificate`cmdlet。   
您需要做的第一件事就是產生 Azure MFA 使用的憑證。  這可以使用 PowerShell。  在本機電腦的憑證存放區中，找到產生的憑證，並已標示包含您 Azure AD directory 的 TenantID 主體名稱。    
  
![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  
  
請注意，TenantID 您 directory 名稱 Azure AD 中。  使用下列 PowerShell cmdlet 產生新的憑證。  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  
      
![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-azure-multi-factor-auth-client-spn"></a>步驟 2：將新的憑證新增至 Azure 多因素驗證 Client SPN   
為了讓 AD FS 伺服器 Azure 多因素驗證 Client 的通訊，您需要新增至 SPN Azure 多因素驗證 Client 認證。 憑證產生使用`New-AdfsAzureMFaTenantCertificate`cmdlet 做為這些認證。 執行下列 PowerShell 使用 Azure 多因素驗證 Client SPN 以新增新的認證。  

>[!NOTE]   
>您需要連接到您的使用 Connect-MsolService PowerShell 使用 Azure AD 的執行個體才能完成此步驟。  這些步驟假設您透過 PowerShell 已連接。  資訊的查看[連接-MsolService。](https://msdn.microsoft.com/library/dn194123.aspx)  
     
  
2. **設定為新認證的憑證針對 Azure 多因素驗證 Client**  
    
    `New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64 `

>[!IMPORTANT]
>  需要上的所有 AD FS 伺服器陣列中都執行這個命令。  Azure AD MFA 將會失敗，已經不需要設定新的認證針對 Azure 多因素驗證 Client 的憑證的伺服器上。 

>[!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 是 Azure 多因素驗證 Client 的 guid。  
  
## <a name="configure-the-ad-fs-farm"></a>設定 AD FS 陣列  
  
一旦您完成在每個 AD FS 伺服器上一節，您將需要執行`Set-AdfsAzureMfaTenant`cmdlet。  
  
這個 cmdlet 需要 AD FS 陣列執行一次。  使用 PowerShell 完成此步驟。    
  
>[!NOTE]  
>您將需要在每個伺服器 AD FS 服務陣列中重新開機，這些變更生效前。  
  
    Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720  
      
![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)  
  
之後，您將會看到 Azure MFA 可為主要的驗證方法內部和外部網路使用。    
  
![AD FS 和 MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>續約及管理 AD FS Azure MFA 憑證
下列指導方針會引導您完成管理 MFA Azure AD FS 伺服器上的憑證的方式。
根據預設，當您設定 AD FS 使用 Azure MFA，憑證產生 New-AdfsAzureMfaTenantCertificate PowerShell cmdlet 透過是有效 2 年。  如何關閉]，以判斷到期您的憑證，以及更新，並安裝新的憑證，使用下列程序。

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>評估 AD FS Azure MFA 憑證到期日
每個 AD FS 伺服器、在本機電腦上我市集中，將會有自簽署的憑證「組織單位 = Microsoft AD FS Azure MFA「發行者主題中。  這是 Azure MFA 的憑證。  檢查有效期間這個判斷到期每個 AD FS 伺服器上的憑證。  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>每個 AD FS 伺服器上建立新的 AD FS Azure MFA 憑證
如果您的憑證的有效期即將結尾，開始更新程序建立新 Azure MFA 伺服器上的憑證每個 AD FS。 在 powershell 命令視窗中，產生使用下列 cmdlet 每個 AD FS 伺服器上新的憑證：

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

根據下列 cmdlet，將產生有效 2 天未來 2 天 + 2 年的新的憑證。  AD FS 和 Azure MFA 作業將不會受到此 cmdlet 或新的憑證。 (請注意：2 日延遲刻意，並提供執行下列步驟來設定新的憑證承租人 AD FS 開始使用 Azure MFA 它之前的時間。)


### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>在 Azure AD 承租人設定的每個新 AD FS Azure MFA 憑證
使用 Azure AD PowerShell 模組」（每個 AD FS 在伺服器上），每個新的憑證更新如下的 Azure AD 承租人設定 (請注意：您必須先連接到使用 Connect-MsolService 執行下列命令)。

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $certbase64
```
    Where $certbase64 is the new certificate.  The base64 encoded certificate can be obtained by exporting the certificate (without the private key) as a DER encoded file and opening in Notepad.exe, then copy/pasting to the PSH session and assigning to the variable $certbase64

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>請確認新的憑證會被用於 Azure MFA
一次新的憑證生效，AD FS 將它們取貨，並開始使用 Azure MFA 幾天的時間在每個各憑證。  當發生這種情形時，每個伺服器您將會看到事件登入 AD FS 管理員事件登入，使用下列資訊：登入名稱：AD FS 管理員日來源：AD FS 日期：2018 年 2 月 27 日 7:33:31 PM 263: 547 工作分類：無層級：資訊關鍵字: AD FS 使用者：DOMAIN\adfssvc 電腦：ADFS.domain.contoso.com 描述：Azure MFA 承租人憑證已經更新。  

TenantId: contoso.onmicrosoft.com。舊指紋：7CC103D60967318A11D8C51C289EF85214D9FC63。 舊的到期日期：2019 年 9 月 15 日 9:43:17 PM。 新的指紋：8110D7415744C9D4D5A4A6309499F7B48B5F3CCF。 新的到期日期：2020 年 2 月 27 日 2: AM 16:07。

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>自訂 AD FS 網頁指南使用者登記 MFA 驗證方法

使用下列範例自訂使用者有不尚未 proofed 上 AD FS 網頁（設定 MFA 驗證資訊）。

### <a name="find-the-error"></a>尋找錯誤
首先，有幾個不同的錯誤訊息會傳回 AD FS 案例中的使用者缺少驗證資訊。
如果您正在使用 Azure MFA 做為主要的驗證，取消 proofed 使用者將會看到包含下列訊息 AD FS 錯誤頁面：
```
    <div id="errorArea"> 
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred 
        </div> 
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information. 
        </div>
```
當您嘗試為額外的驗證 Azure AD 時，未 proofed 使用者將會看到包含下列訊息 AD FS 錯誤頁面：
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

### <a name="catch-the-error-and-update-the-page-text"></a>捕捉錯誤和更新的頁面上的文字
捕捉錯誤，並顯示使用者自訂指導方針其實 javascript 附加到結尾 onload.js 檔案的一部分 AD FS web（1）搜尋辨識錯誤字串，並提供自訂（2）主題網頁。  (如何自訂 onload.js 檔案的一般指導方針，會看到文章[在此](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/advanced-customization-of-ad-fs-sign-in-pages)。)

