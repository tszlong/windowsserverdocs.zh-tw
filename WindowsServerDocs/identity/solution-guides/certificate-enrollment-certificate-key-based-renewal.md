---
title: 在自訂埠上為憑證金鑰型更新設定憑證註冊 Web 服務
description: 深入瞭解：在自訂埠上設定憑證金鑰型更新的憑證註冊 Web 服務
author: Deland-Han
ms.author: delhan
manager: dcscontentpm
ms.date: 11/12/2019
ms.topic: article
ms.openlocfilehash: 55ac25e37f7c7621426db031ba1ad148d3a02a98
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042396"
---
# <a name="configuring-certificate-enrollment-web-service-for-certificate-key-based-renewal-on-a-custom-port"></a>在自訂埠上為憑證金鑰型更新設定憑證註冊 Web 服務

> 作者： Jitesh Thakur、Meera Mohideen、Windows 群組的技術顧問。
Windows 群組的 Ankit Tyagi 支援工程師

## <a name="summary"></a>摘要

本文提供逐步指示，說明如何在443以外的自訂埠上執行憑證註冊原則 Web 服務 (CEP) 和憑證註冊 Web 服務 (CES) ，以利用 CEP 和 CES 的自動續約功能。

本文也說明 CEP 和 CES 的運作方式，並提供安裝指導方針。

> [!Note]
> 本文所包含的工作流程適用于特定案例。 相同的工作流程在不同的情況下可能無法運作。 不過，原則仍維持不變。
>
> 免責聲明：此設定是針對特定需求所建立，而您不想要針對 CEP 和 CES 伺服器使用通訊埠443進行預設的 HTTPS 通訊。 雖然這是可行的設定，但是它的支援能力有限。 如果您遵循本指南，請謹慎地使用來自所提供 web 伺服器設定的最小偏差，客戶服務和支援人員可提供最大的協助。

## <a name="scenario"></a>案例

在此範例中，指示是以使用下列設定的環境為基礎：

- 具有 Active Directory 憑證服務 (AD CS) 公開金鑰基礎結構 (PKI) 的 Contoso.com 樹系。

- 在服務帳戶下執行的一部伺服器上設定的兩個 CEP/CES 實例。 一個實例會使用使用者名稱和密碼進行初始註冊。 另一個則使用以憑證為基礎的驗證，在僅限更新模式中進行以金鑰為基礎的更新。

- 使用者擁有工作組或未加入網域的電腦，其會使用使用者名稱和密碼認證來註冊電腦憑證。

- 從使用者到 CEP 的連線，以及透過 HTTPS 的 CES 都會發生在自訂埠，例如49999。  (此埠是從動態埠範圍選取的，而且不會由任何其他服務當做靜態埠使用。 ) 

- 當憑證存留期即將結束時，電腦會使用以憑證為基礎的 CES 金鑰型更新來更新相同通道上的憑證。

![部署](media/certificate-enrollment-certificate-key-based-renewal-1.png)

## <a name="configuration-instructions"></a>設定指示

### <a name="overview"></a>概觀

1. 設定金鑰型更新的範本。

2. 先決條件是設定 CEP 和 CES 伺服器，以進行使用者名稱和密碼驗證。
   在此環境中，我們將實例稱為 "CEPCES01"。

3.  針對相同伺服器上的憑證型驗證，使用 PowerShell 來設定另一個 CEP 和 CES 實例。 CES 實例將使用服務帳戶。

    在此環境中，我們將實例稱為 "CEPCES02"。 使用的服務帳戶是 "cepcessvc"。

4.  設定用戶端設定。

### <a name="configuration"></a>組態

本節提供設定初始註冊的步驟。

> [!Note]
> 您也可以設定任何使用者服務帳戶、MSA 或 GMSA，讓 CES 可以運作。

先決條件是，您必須使用使用者名稱和密碼驗證，在伺服器上設定 CEP 和 CES。

#### <a name="configure-the-template-for-key-based-renewal"></a>設定範本以進行金鑰型更新

您可以複製現有的電腦範本，並設定範本的下列設定：

1. 在憑證範本的 [主體名稱] 索引標籤上，確定已選取 [ **在要求中提供** 並 **使用現有憑證的主體資訊進行自動註冊更新要求** ] 選項。
   ![新範本](media/certificate-enrollment-certificate-key-based-renewal-2.png)

2. 切換至 [ **發佈需求** ] 索引標籤，然後選取 [ **CA 憑證管理員核准** ] 核取方塊。
   ![發佈需求](media/certificate-enrollment-certificate-key-based-renewal-3.png)

3. 將 [ **讀取** ] 和 [ **註冊** ] 許可權指派給此範本的 **cepcessvc** 服務帳戶。

4. 在 CA 上發佈新的範本。

> [!Note]
> 請確定範本上的相容性設定已設定為 **Windows server 2012 R2** ，因為如果相容性設定為 windows server 2016 或更新版本，則不會顯示範本的已知問題。 如需詳細資訊，請參閱 [無法從 Windows server 2016 或更新版本的 ca 或 CEP 伺服器選取 Windows server 2016 與 CA 相容的憑證範本](https://support.microsoft.com/en-in/help/4508802/cannot-select-certificate-templates-in-windows-server-2016)。


#### <a name="configure-the-cepces01-instance"></a>設定 CEPCES01 實例

##### <a name="step-1-install-the-instance"></a>步驟1：安裝實例

若要安裝 CEPCES01 實例，請使用下列其中一種方法。

**方法 1**

請參閱下列文章，以取得啟用 CEP 和 CES 以進行使用者名稱和密碼驗證的逐步指引：

[憑證註冊原則 Web 服務指導方針](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831625(v=ws.11))

[憑證註冊 Web 服務指導方針](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831822(v=ws.11)#configure-a-ca-for-the-certificate-enrollment-web-service)

> [!Note]
> 如果您同時設定使用者名稱和密碼驗證的 CEP 和 CES 實例，請確定未選取 [啟用 Key-Based 續約] 選項。

**方法 2**

您可以使用下列 PowerShell Cmdlet 來安裝 CEP 和 CES 實例：

```PowerShell
Import-Module ServerManager
Add-WindowsFeature Adcs-Enroll-Web-Pol
Add-WindowsFeature Adcs-Enroll-Web-Svc
```

```PowerShell
Install-AdcsEnrollmentPolicyWebService -AuthenticationType Username -SSLCertThumbprint "sslCertThumbPrint"
```

此命令會藉由指定用於驗證的使用者名稱和密碼，來安裝憑證註冊原則 Web 服務 (CEP) 。

> [!Note]
> 在此命令中， \<**SSLCertThumbPrint**\> 是將用來系結 IIS 之憑證的憑證指紋。

```PowerShell
Install-AdcsEnrollmentWebService -ApplicationPoolIdentity -CAConfig "CA1.contoso.com\contoso-CA1-CA" -SSLCertThumbprint "sslCertThumbPrint" -AuthenticationType Username
```

此命令會安裝憑證註冊 Web 服務 (CES) ，以使用 **CA1.contoso.com** 電腦名稱稱的憑證授權單位單位以及 **CONTOSO-CA1 ca** 的 ca 一般名稱。 CES 的身分識別會指定為預設的應用程式集區身分識別。 驗證類型為 **username**。 SSLCertThumbPrint 是將用來系結 IIS 之憑證的指紋。

##### <a name="step-2-check-the-internet-information-services-iis-manager-console"></a>步驟2檢查 Internet Information Services (IIS) 管理員主控台

安裝成功之後，您預期會在 Internet Information Services (IIS) Manager 主控台中看到下列顯示。
![IIS 管理員](media/certificate-enrollment-certificate-key-based-renewal-4.png)

在 [ **預設的網站**] 底下，選取 [ **ADPolicyProvider_CEP_UsernamePassword**]，然後開啟 [ **應用程式設定**]。 請記下 **識別碼** 和 **URI**。

您可以為管理新增 **易記名稱** 。

#### <a name="configure-the-cepces02-instance"></a>設定 CEPCES02 實例

##### <a name="step-1-install-the-cep-and-ces-for-key-based-renewal-on-the-same-server"></a>步驟1：在同一部伺服器上安裝 CEP 和 CES 以進行金鑰型續約。

在 PowerShell 中執行下列命令：

```PowerShell
Install-AdcsEnrollmentPolicyWebService -AuthenticationType Certificate -SSLCertThumbprint "sslCertThumbPrint" -KeyBasedRenewal
```

此命令會)  (CEP 安裝憑證註冊原則 Web 服務，並指定使用憑證進行驗證。

> [!Note]
> 在此命令中， \<SSLCertThumbPrint\> 是將用來系結 IIS 之憑證的憑證指紋。

以金鑰為基礎的更新可讓憑證用戶端使用其現有憑證的金鑰來更新其憑證，以進行驗證。 在以金鑰為基礎的續約模式中，服務只會傳回針對金鑰型更新所設定的憑證範本。

```PowerShell
Install-AdcsEnrollmentWebService -CAConfig "CA1.contoso.com\contoso-CA1-CA" -SSLCertThumbprint "sslCertThumbPrint" -AuthenticationType Certificate -ServiceAccountName "Contoso\cepcessvc" -ServiceAccountPassword (read-host "Set user password" -assecurestring) -RenewalOnly -AllowKeyBasedRenewal
```

此命令會安裝憑證註冊 Web 服務 (CES) ，以使用 **CA1.contoso.com** 電腦名稱稱的憑證授權單位單位以及 **CONTOSO-CA1 ca** 的 ca 一般名稱。

在此命令中，會將憑證註冊 Web 服務的身分識別指定為 **cepcessvc** 服務帳戶。 驗證類型為 **certificate**。 **SSLCertThumbPrint** 是將用來系結 IIS 之憑證的指紋。

**RenewalOnly** Cmdlet 可讓 CES 在僅限更新模式中執行。 **AllowKeyBasedRenewal** Cmdlet 也會指定 CES 接受註冊伺服器的金鑰型更新要求。 這些是不會直接對應至安全性主體之驗證的有效用戶端憑證。

> [!Note]
> 服務帳戶必須是伺服器上 **IISUsers** 群組的一部分。

##### <a name="step-2-check-the-iis-manager-console"></a>步驟2檢查 IIS 管理員主控台

安裝成功之後，您預期會在 [IIS 管理員] 主控台中看到下列顯示。
![IIS 管理員](media/certificate-enrollment-certificate-key-based-renewal-5.png)

選取 [**預設的網站**] 底下的 **KeyBasedRenewal_ADPolicyProvider_CEP_Certificate** ，然後開啟 [**應用程式設定**]。 記下 **識別碼** 和 **URI**。 您可以為管理新增 **易記名稱** 。

> [!Note]
> 如果實例是安裝在新的伺服器上，請再次檢查識別碼，以確定識別碼與 CEPCES01 實例中所產生的識別碼相同。 如果值不同，您可以直接複製並貼上該值。

#### <a name="complete-certificate-enrollment-web-services-configuration"></a>完成憑證註冊 Web 服務設定

若要能夠代表 CEP 和 CES 的功能來註冊憑證，您必須在 Active Directory 中設定工作組的電腦帳戶，然後在服務帳戶上設定限制委派。

##### <a name="step-1-create-a-computer-account-of-the-workgroup-computer-in-active-directory"></a>步驟1：在 Active Directory 中建立工作組電腦的電腦帳戶

此帳戶將用來進行以金鑰為基礎的更新，以及憑證範本上的 [發佈至 Active Directory] 選項的驗證。

> [!Note]
> 您不需要將用戶端電腦加入網域。 此帳戶在 KBR for dsmapper service 中執行以憑證為基礎的驗證時，會變成一張圖片。

![New Object](media/certificate-enrollment-certificate-key-based-renewal-6.png)

##### <a name="step-2-configure-the-service-account-for-constrained-delegation-s4u2self"></a>步驟2：將服務帳戶設定為限制委派 (S4U2Self) 

執行下列 PowerShell 命令來啟用限制委派 (S4U2Self 或任何驗證通訊協定) ：

```PowerShell
Get-ADUser -Identity cepcessvc | Set-ADAccountControl -TrustedToAuthForDelegation $True
Set-ADUser -Identity cepcessvc -Add @{'msDS-AllowedToDelegateTo'=@('HOST/CA1.contoso.com','RPCSS/CA1.contoso.com')}
```

> [!Note]
> 在此命令中， \<cepcessvc\> 是服務帳戶，而 <CA1.contoso.com >是憑證授權單位單位。

> [!Important]
> 我們不會在此設定的 CA 上啟用 RENEWALONBEHALOF 旗標，因為我們使用限制委派來為我們進行相同的工作。 這可讓我們避免將服務帳戶的許可權新增至 CA 的安全性。

##### <a name="step-3-configure-a-custom-port-on-the-iis-web-server"></a>步驟3：在 IIS 網頁伺服器上設定自訂埠

1. 在 [IIS 管理員] 主控台中，選取 [預設的網站]。

2. 在 [動作] 窗格中，選取 [編輯網站系結]。

3. 將預設埠設定從443變更為您的自訂埠。 範例螢幕擷取畫面顯示埠設定為49999。
   ![變更埠](media/certificate-enrollment-certificate-key-based-renewal-7.png)

##### <a name="step-4-edit-the-ca-enrollment-services-object-on-active-directory"></a>步驟4：編輯 Active Directory 上的 CA 註冊服務物件

1. 在網域控制站上，開啟 adsiedit。

2. [連接到設定磁碟分割](/previous-versions/windows/it-pro/windows-server-2003/ff730188(v=ws.10))，然後流覽至您的 CA 註冊服務物件：

   CN = ENTCA，CN = 註冊服務，CN = Public Key Services，CN = Services，CN = Configuration，DC = contoso，DC = com

3. 以滑鼠右鍵按一下並編輯 CA 物件。 使用自訂埠搭配您的 CEP 和 CES 伺服器 Uri （在應用程式設定中找到）來變更 **mspki-certificate-name-flag-註冊-伺服器** 屬性。 例如：

   ```
   140https://cepces.contoso.com:49999/ENTCA_CES_UsernamePassword/service.svc/CES0
   181https://cepces.contoso.com:49999/ENTCA_CES_Certificate/service.svc/CES1
   ```

   ![ADSI 編輯器](media/certificate-enrollment-certificate-key-based-renewal-8.png)

#### <a name="configure-the-client-computer"></a>設定用戶端電腦

在用戶端電腦上，設定註冊原則和自動註冊原則。 若要這樣做，請遵循下列步驟：

1. 選取 [**開始**  >  **執行**]，然後輸入 **gpedit.msc**。

2. 移至 [**電腦**  >  **設定 Windows 設定**  >  **安全性設定**]，然後按一下 [**公開金鑰原則**]。

3. 啟用 [ **憑證服務用戶端-自動註冊] 原則** ，以符合下列螢幕擷取畫面中的設定。
   ![憑證群組原則](media/certificate-enrollment-certificate-key-based-renewal-9.png)

4. 啟用 **憑證服務用戶端憑證註冊原則**。

   a. 按一下 [ **新增** ] 以新增註冊原則，並使用我們在 ADSI 中編輯的 **USERNAMEPASSWORD** 輸入 CEP URI。

   b. 針對 [ **驗證類型**]，選取 [使用者 **名稱/密碼**]。

   c. 將優先權設定為 **10**，然後驗證原則伺服器。
      ![註冊原則](media/certificate-enrollment-certificate-key-based-renewal-10.png)

   > [!Note]
   > 請確定埠號碼已新增至 URI，而且可在防火牆上使用。

5. 透過 certlm.msc 註冊電腦的第一個憑證。
   ![註冊原則](media/certificate-enrollment-certificate-key-based-renewal-11.png)

   選取 KBR 範本並註冊憑證。
   ![註冊原則](media/certificate-enrollment-certificate-key-based-renewal-12.png)

6. 再次開啟 **gpedit.msc** 。 編輯 [ **憑證服務用戶端-憑證註冊原則**]，然後新增以金鑰為基礎的更新註冊原則：

   a. 按一下 [ **新增**]，輸入在 ADSI 中編輯之 **憑證** 的 CEP URI。

   b. 將優先順序設定為 **1**，然後驗證原則伺服器。 系統會提示您進行驗證，並選擇我們最初註冊的憑證。

   ![註冊原則](media/certificate-enrollment-certificate-key-based-renewal-13.png)

> [!Note]
> 請確定金鑰型續約註冊原則的優先順序值低於使用者名稱密碼註冊原則優先權的優先順序。 第一個喜好設定會給予最低優先順序。

## <a name="testing-the-setup"></a>測試設定

若要確認自動更新是否正常運作，請使用 mmc 以相同的金鑰更新憑證，以確認手動更新是否正常運作。 此外，當您更新時，系統應該會提示您選取憑證。 您可以選擇我們稍早註冊的憑證。 需要提示。

開啟「電腦個人」憑證存放區，並新增「封存的憑證」視圖。 若要這樣做，請將本機電腦帳戶嵌入式管理單元新增至 mmc.exe、將憑證反白顯示 **(本機電腦)** 按一下該檔案， **然後從 mmc** 右側或頂端的 [ **動作]** 索引標籤中按一下 [流覽]，再按一下 [ **視圖選項**]，選取 [封存的 **憑證**]，然後按一下 **[確定]**。

### <a name="method-1"></a>方法 1

執行以下命令：

```PowerShell
certreq -machine -q -enroll -cert <thumbprint> renew
```

![命令](media/certificate-enrollment-certificate-key-based-renewal-14.png)

### <a name="method-2"></a>方法 2

將用戶端電腦的時間和日期前移至憑證範本的更新時間。

例如，憑證範本有2天的有效性設定，並設定了8小時的更新設定。 範例憑證于上午4:00 發行。 在當月的第18天，于上午4:00 到期 在20。 自動註冊引擎會在重新開機時觸發，並每隔8小時間隔 (大約) 。

因此，如果您將時間前移至 8:10 P.M。 在19，因為在範本上的更新時間範圍設定為8小時，所以執行 Certutil-脈衝 (來觸發 AE 引擎) 為您註冊憑證。

![命令](media/certificate-enrollment-certificate-key-based-renewal-15.png)

測試完成後，請將時間設定還原為原始值，然後重新開機用戶端電腦。

> [!Note]
> 先前的螢幕擷取畫面範例示範自動註冊引擎如預期般運作，因為 CA 日期仍設為18。 因此，它會繼續發出憑證。 在現實生活的情況下，將不會進行這項大量的續約。

## <a name="references"></a>參考資料

[測試實驗室指南：示範憑證金鑰型更新](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj590165(v%3dws.11))

[憑證註冊 Web 服務](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Certificate-Enrollment-Web-Services/ba-p/397385)

[安裝-AdcsEnrollmentPolicyWebService](/powershell/module/adcsdeployment/install-adcsenrollmentpolicywebservice)

[安裝-AdcsEnrollmentWebService](/powershell/module/adcsdeployment/install-adcsenrollmentwebservice)

另請參閱

[Windows Server 安全性論壇](https://aka.ms/adcsforum)

[Active Directory 憑證服務 (AD CS) 公開金鑰基礎結構 (PKI) 常見問題集 (FAQ)](https://aka.ms/adcsfaq)

[Windows PKI 文件參考和程式庫](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/windows-pki-documentation-reference/ba-p/1128393)

[Windows PKI 部落格](/archive/blogs/pki/)

[如何在 Web 註冊 proxy 頁面的自訂服務帳戶上設定 Kerberos 限制委派 (S4U2Proxy 或 Kerberos) ](https://support.microsoft.com/help/4494313/configuring-web-enrollment-proxy-for-s4u2proxy-constrained-delegation)
