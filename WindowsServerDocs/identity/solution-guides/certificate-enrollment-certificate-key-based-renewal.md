---
title: 在自訂埠上為憑證金鑰型更新設定憑證註冊 Web 服務
author: Deland-Han
ms.author: delhan
manager: dcscontentpm
ms.date: 11/12/2019
ms.topic: article
ms.prod: windows-server
ms.openlocfilehash: c4c74fc5fef01c21d5c1818c212c004786caca66
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87182214"
---
# <a name="configuring-certificate-enrollment-web-service-for-certificate-key-based-renewal-on-a-custom-port"></a>在自訂埠上為憑證金鑰型更新設定憑證註冊 Web 服務

> 作者： Jitesh Thakur、Meera Mohideen、Windows Group 的技術顧問。
Windows 群組的 Ankit Tyagi 支援工程師

## <a name="summary"></a>總結

本文提供逐步指示，讓您在443以外的自訂埠上執行憑證註冊原則 Web 服務（CEP）和憑證註冊 Web 服務（CES）以進行憑證金鑰型更新，以利用 CEP 和 CES 的自動續約功能。

本文也會說明 CEP 和 CES 的運作方式，並提供安裝指導方針。

> [!Note]
> 本文中所包含的工作流程適用于特定案例。 相同的工作流程在不同的情況下可能無法使用。 不過，原則會維持不變。
>
> 免責聲明：針對 CEP 和 CES 伺服器的預設 HTTPS 通訊，您不想使用埠443的特定需求，會建立此設定。 雖然這是可行的設定，但它具有有限的支援能力。 如果您使用所提供的 web 伺服器設定的最小偏差，則客戶服務及支援人員最能協助您。

## <a name="scenario"></a>狀況

在此範例中，指示是以使用下列設定的環境為基礎：

- 具有 Active Directory 憑證服務（AD CS）公開金鑰基礎結構（PKI）的 Contoso.com 樹系。

- 在服務帳戶下執行的一部伺服器上設定的兩個 CEP/CES 實例。 一個實例會使用使用者名稱和密碼進行初始註冊。 另一個則使用以憑證為基礎的驗證，用於僅限更新模式中的金鑰型更新。

- 使用者有一個工作組或未加入網域的電腦，他將使用使用者名稱和密碼認證來註冊電腦憑證。

- 從使用者到 CEP 和 CES over HTTPS 的連線會發生在自訂埠（例如49999）上。 （此埠是從動態埠範圍選取，而且不會由任何其他服務當做靜態埠使用）。

- 當憑證存留期即將結束時，電腦會使用以憑證為基礎的 CES 金鑰型更新，以透過相同的通道更新憑證。

![部署](media/certificate-enrollment-certificate-key-based-renewal-1.png)

## <a name="configuration-instructions"></a>設定指示

### <a name="overview"></a>概觀

1. 設定金鑰型更新的範本。

2. 必要條件是，設定 CEP 和 CES 伺服器以進行使用者名稱和密碼驗證。
   在此環境中，我們將實例稱為「CEPCES01」。

3.  在同一部伺服器上使用 PowerShell 來設定另一個 CEP 和 CES 實例，以進行以憑證為基礎的驗證。 CES 實例會使用服務帳戶。

    在此環境中，我們將實例稱為「CEPCES02」。 使用的服務帳戶是 "cepcessvc"。

4.  設定用戶端設定。

### <a name="configuration"></a>組態

本節提供設定初始註冊的步驟。

> [!Note]
> 您也可以設定任何使用者服務帳戶、MSA 或 GMSA，讓 CES 能夠正常執行。

必要條件是，您必須使用使用者名稱和密碼驗證，在伺服器上設定 CEP 和 CES。

#### <a name="configure-the-template-for-key-based-renewal"></a>設定金鑰型更新的範本

您可以複製現有的電腦範本，並設定下列範本設定：

1. 在憑證範本的 [主體名稱] 索引標籤上，確認已選取 [**在要求中提供**] 和 [**從現有憑證使用主旨資訊來進行自動註冊更新要求**] 選項。
   ![新範本](media/certificate-enrollment-certificate-key-based-renewal-2.png)

2. 切換至 [**發佈需求**] 索引標籤，然後選取 [ **CA 憑證管理員核准**] 核取方塊。
   ![發行需求](media/certificate-enrollment-certificate-key-based-renewal-3.png)

3. 將此範本的 [**讀取**] 和 [**註冊**] 許可權指派給**cepcessvc**服務帳戶。

4. 在 CA 上發佈新範本。

> [!Note]
> 請確定範本上的相容性設定是設為**Windows server 2012 R2** ，因為已知的問題是，如果相容性設定為 Windows server 2016 或更新版本，則不會顯示範本。 如需詳細資訊，請參閱[無法從 Windows server 2016 或更新版本的 ca 或 CEP 伺服器選取 Windows server 2016 與 CA 相容的憑證範本](https://support.microsoft.com/en-in/help/4508802/cannot-select-certificate-templates-in-windows-server-2016)。


#### <a name="configure-the-cepces01-instance"></a>設定 CEPCES01 實例

##### <a name="step-1-install-the-instance"></a>步驟1：安裝實例

若要安裝 CEPCES01 實例，請使用下列其中一種方法。

**方法 1**

如需針對使用者名稱和密碼驗證啟用 CEP 和 CES 的逐步指引，請參閱下列文章：

[憑證註冊原則 Web 服務指導方針](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831625(v=ws.11))

[憑證註冊 Web 服務指導方針](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831822(v=ws.11)#configure-a-ca-for-the-certificate-enrollment-web-service)

> [!Note]
> 如果您同時設定 CEP 和 CES 實例的使用者名稱和密碼驗證，請確定您未選取 [啟用金鑰型更新] 選項。

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

此命令會藉由指定使用者名稱和密碼來進行驗證，以安裝憑證註冊原則 Web 服務（CEP）。

> [!Note]
> 在此命令中， \<**SSLCertThumbPrint**\> 是將用來系結 IIS 之憑證的指紋。

```PowerShell
Install-AdcsEnrollmentWebService -ApplicationPoolIdentity -CAConfig "CA1.contoso.com\contoso-CA1-CA" -SSLCertThumbprint "sslCertThumbPrint" -AuthenticationType Username
```

此命令會安裝憑證註冊 Web 服務（CES）以使用電腦名稱稱為**CA1.contoso.com**的憑證授權單位單位，以及**CA1**ca 的一般名稱。 CES 的識別會指定為預設的應用程式集區身分識別。 [驗證類型] 為 [使用者**名稱**]。 SSLCertThumbPrint 是將用來系結 IIS 之憑證的指紋。

##### <a name="step-2-check-the-internet-information-services-iis-manager-console"></a>步驟2檢查 Internet Information Services （IIS）管理員主控台

成功安裝之後，您應該會在 Internet Information Services （IIS）管理員主控台中看到下列顯示畫面。
![IIS 管理員](media/certificate-enrollment-certificate-key-based-renewal-4.png)

在 [**預設的網站**] 底下，選取 [ **ADPolicyProvider_CEP_UsernamePassword**]，然後開啟 [**應用程式設定**]。 請記下**識別碼**和**URI**。

您可以新增**易記名稱**來進行管理。

#### <a name="configure-the-cepces02-instance"></a>設定 CEPCES02 實例

##### <a name="step-1-install-the-cep-and-ces-for-key-based-renewal-on-the-same-server"></a>步驟1：在相同的伺服器上安裝 CEP 和 CES 以進行金鑰型更新。

在 PowerShell 中執行下列命令：

```PowerShell
Install-AdcsEnrollmentPolicyWebService -AuthenticationType Certificate -SSLCertThumbprint "sslCertThumbPrint" -KeyBasedRenewal
```

此命令會安裝憑證註冊原則 Web 服務（CEP），並指定使用憑證進行驗證。

> [!Note]
> 在此命令中， \<SSLCertThumbPrint\> 是將用來系結 IIS 之憑證的指紋。

金鑰型更新可讓憑證用戶端使用其現有憑證的金鑰來更新憑證，以進行驗證。 在以金鑰為基礎的更新模式中，服務只會傳回針對金鑰型更新所設定的憑證範本。

```PowerShell
Install-AdcsEnrollmentWebService -CAConfig "CA1.contoso.com\contoso-CA1-CA" -SSLCertThumbprint "sslCertThumbPrint" -AuthenticationType Certificate -ServiceAccountName "Contoso\cepcessvc" -ServiceAccountPassword (read-host "Set user password" -assecurestring) -RenewalOnly -AllowKeyBasedRenewal
```

此命令會安裝憑證註冊 Web 服務（CES）以使用電腦名稱稱為**CA1.contoso.com**的憑證授權單位單位，以及**CA1**ca 的一般名稱。

在此命令中，憑證註冊 Web 服務的身分識別會指定為**cepcessvc**服務帳戶。 驗證類型為**certificate**。 **SSLCertThumbPrint**是將用來系結 IIS 之憑證的指紋。

**RenewalOnly** Cmdlet 可讓 CES 在僅限更新模式中執行。 **AllowKeyBasedRenewal** Cmdlet 也會指定 CES 會接受註冊伺服器的金鑰型更新要求。 這些是有效的用戶端憑證，用於不會直接對應至安全性主體的驗證。

> [!Note]
> 服務帳戶必須是伺服器上**IISUsers**群組的一部分。

##### <a name="step-2-check-the-iis-manager-console"></a>步驟2檢查 IIS 管理員主控台

成功安裝之後，您應該會在 IIS 管理員主控台中看到下列顯示畫面。
![IIS 管理員](media/certificate-enrollment-certificate-key-based-renewal-5.png)

選取 [**預設的網站**] 底下的**KeyBasedRenewal_ADPolicyProvider_CEP_Certificate** ，然後開啟 [**應用程式設定**]。 記下 [**識別碼**] 和 [ **URI**]。 您可以新增**易記名稱**來進行管理。

> [!Note]
> 如果實例安裝在新的伺服器上，請再次檢查識別碼，以確定識別碼與 CEPCES01 實例中產生的識別碼相同。 如果值不同，您可以直接複製並貼上該值。

#### <a name="complete-certificate-enrollment-web-services-configuration"></a>完成憑證註冊 Web 服務設定

若要能夠代表 CEP 和 CES 的功能來註冊憑證，您必須在 Active Directory 中設定工作組的電腦帳戶，然後在服務帳戶上設定限制委派。

##### <a name="step-1-create-a-computer-account-of-the-workgroup-computer-in-active-directory"></a>步驟1：在 Active Directory 中建立工作組電腦的電腦帳戶

此帳戶將用於驗證以金鑰為基礎的更新，以及憑證範本上的 [發佈至 Active Directory] 選項。

> [!Note]
> 您不需要加入用戶端電腦的網域。 在 KBR for dsmapper 服務中進行憑證型驗證時，此帳戶會進入圖片。

![New Object](media/certificate-enrollment-certificate-key-based-renewal-6.png)

##### <a name="step-2-configure-the-service-account-for-constrained-delegation-s4u2self"></a>步驟2：設定限制委派的服務帳戶（S4U2Self）

執行下列 PowerShell 命令以啟用限制委派（S4U2Self 或任何驗證通訊協定）：

```PowerShell
Get-ADUser -Identity cepcessvc | Set-ADAccountControl -TrustedToAuthForDelegation $True
Set-ADUser -Identity cepcessvc -Add @{'msDS-AllowedToDelegateTo'=@('HOST/CA1.contoso.com','RPCSS/CA1.contoso.com')}
```

> [!Note]
> 在此命令中， \<cepcessvc\> 是服務帳戶，而 <CA1.contoso.com >是憑證授權單位單位。

> [!Important]
> 我們不會在此設定中于 CA 上啟用 RENEWALONBEHALOF 旗標，因為我們使用限制委派來為我們執行相同的工作。 這可讓我們避免將服務帳戶的許可權新增至 CA 的安全性。

##### <a name="step-3-configure-a-custom-port-on-the-iis-web-server"></a>步驟3：在 IIS web 伺服器上設定自訂埠

1. 在 IIS 管理員主控台中，選取 [預設的網站]。

2. 在 [動作] 窗格中，選取 [編輯網站系結]。

3. 將預設通訊埠設定從443變更為您的自訂埠。 範例螢幕擷取畫面顯示通訊埠設定為49999。
   ![變更埠](media/certificate-enrollment-certificate-key-based-renewal-7.png)

##### <a name="step-4-edit-the-ca-enrollment-services-object-on-active-directory"></a>步驟4：編輯 Active Directory 上的 CA 註冊服務物件

1. 在網域控制站上，開啟 [adsiedit]。

2. [連接到設定磁碟分割](/previous-versions/windows/it-pro/windows-server-2003/ff730188(v=ws.10))，並流覽至您的 CA 註冊服務物件：

   CN = ENTCA，CN = 註冊服務，CN = Public Key Services，CN = Services，CN = Configuration，DC = contoso，DC = com

3. 以滑鼠右鍵按一下並編輯 CA 物件。 使用自訂埠搭配您在 [應用程式設定] 中找到的 CEP 和 CES 伺服器 Uri，以變更 [ **mspki-site-name-註冊-伺服器**] 屬性。 例如：

   ```
   140https://cepces.contoso.com:49999/ENTCA_CES_UsernamePassword/service.svc/CES0
   181https://cepces.contoso.com:49999/ENTCA_CES_Certificate/service.svc/CES1
   ```

   ![ADSI 編輯器](media/certificate-enrollment-certificate-key-based-renewal-8.png)

#### <a name="configure-the-client-computer"></a>設定用戶端電腦

在用戶端電腦上，設定註冊原則和自動註冊原則。 若要這樣做，請執行下列步驟：

1. 選取 [**開始**  >  **執行**]，然後輸入**gpedit.msc**。

2. 移至 [**電腦**設定] [  >  **Windows 設定**] [  >  **安全性設定**]，然後按一下 [**公開金鑰原則**]。

3. 啟用 [**憑證服務用戶端-自動註冊] 原則**，以符合下列螢幕擷取畫面中的設定。
   ![憑證群組原則](media/certificate-enrollment-certificate-key-based-renewal-9.png)

4. 啟用**憑證服務用戶端憑證註冊原則**。

   a. 按一下 [**新增**] 以新增註冊原則，並輸入我們在 ADSI 中編輯的**USERNAMEPASSWORD**的 CEP URI。

   b. 針對 [**驗證類型**]，選取 [使用者**名稱/密碼**]。

   c. 設定**10**的優先順序，然後驗證原則伺服器。
      ![註冊原則](media/certificate-enrollment-certificate-key-based-renewal-10.png)

   > [!Note]
   > 請確定埠號碼已新增至 URI，且可在防火牆上使用。

5. 透過 certlm.msc 註冊電腦的第一個憑證。
   ![註冊原則](media/certificate-enrollment-certificate-key-based-renewal-11.png)

   選取 [KBR] 範本並註冊憑證。
   ![註冊原則](media/certificate-enrollment-certificate-key-based-renewal-12.png)

6. 再次開啟**gpedit.msc** 。 編輯 [**憑證服務用戶端-憑證註冊原則**]，然後新增以金鑰為基礎的更新註冊原則：

   a. 按一下 [**新增**]，輸入我們在 ADSI 中編輯的**憑證**的 CEP URI。

   b. 將優先順序設定為**1**，然後驗證原則伺服器。 系統會提示您進行驗證，並選擇我們最初註冊的憑證。

   ![註冊原則](media/certificate-enrollment-certificate-key-based-renewal-13.png)

> [!Note]
> 請確定以金鑰為基礎之更新註冊原則的優先順序值低於使用者名稱密碼註冊原則優先順序的優先順序。 第一個喜好設定會給予最低優先順序。

## <a name="testing-the-setup"></a>測試設定

若要確定自動續約運作正常，請使用 mmc 更新具有相同金鑰的憑證，以確認手動更新是否正常運作。 此外，在更新時，系統應該會提示您選取憑證。 您可以選擇我們稍早註冊的憑證。 需要提示。

開啟 [電腦個人] 憑證存放區，然後新增 [封存的憑證] 視圖。 若要這麼做，請將 [本機電腦帳戶] 嵌入式管理單元新增至 mmc.exe，按一下 [**憑證（本機電腦）** ]，按一下右邊的 [**動作]** 索引標籤或 mmc 頂端的 [**流覽**]，按一下 [**流覽選項**]，選取 [封存的**憑證**]，然後按一下 **[確定]**。

### <a name="method-1"></a>方法 1

執行下列命令：

```PowerShell
certreq -machine -q -enroll -cert <thumbprint> renew
```

![命令](media/certificate-enrollment-certificate-key-based-renewal-14.png)

### <a name="method-2"></a>方法 2

將用戶端電腦上的時間和日期前移到憑證範本的更新時間。

例如，憑證範本已設定了2天的有效性設定和8小時的更新設定。 範例憑證會在上午4:00 發行 在當月第18天，于上午4:00 到期 20。 自動註冊引擎會在重新開機時觸發，並每隔8小時進行一次（大約）。

因此，如果您將時間前進到下午8:10 在19，因為我們的更新視窗在範本上設定為8小時，所以執行 Certutil-脈衝（以觸發 AE 引擎）會為您註冊憑證。

![命令](media/certificate-enrollment-certificate-key-based-renewal-15.png)

測試完成之後，將時間設定還原為原始值，然後重新開機用戶端電腦。

> [!Note]
> 先前的螢幕擷取畫面是示範自動註冊引擎如預期般運作的範例，因為 CA 日期仍然設定為18。 因此，它會繼續發行憑證。 在實際情況下，將不會進行這項大量的更新。

## <a name="references"></a>參考資料

[測試實驗室指南：示範憑證金鑰型更新](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj590165(v%3dws.11))

[憑證註冊 Web 服務](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Certificate-Enrollment-Web-Services/ba-p/397385)

[安裝-AdcsEnrollmentPolicyWebService](/powershell/module/adcsdeployment/install-adcsenrollmentpolicywebservice?view=win10-ps)

[安裝-AdcsEnrollmentWebService](/powershell/module/adcsdeployment/install-adcsenrollmentwebservice?view=win10-ps)

另請參閱

[Windows Server 安全性論壇](https://aka.ms/adcsforum)

[Active Directory 憑證服務 (AD CS) 公開金鑰基礎結構 (PKI) 常見問題集 (FAQ)](https://aka.ms/adcsfaq)

[Windows PKI 文件參考和程式庫](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/windows-pki-documentation-reference/ba-p/1128393)

[Windows PKI 部落格](/archive/blogs/pki/)

[如何在網頁註冊 proxy 頁面的自訂服務帳戶上設定 Kerberos 限制委派（僅限 S4U2Proxy 或 Kerberos）](https://support.microsoft.com/help/4494313/configuring-web-enrollment-proxy-for-s4u2proxy-constrained-delegation)
