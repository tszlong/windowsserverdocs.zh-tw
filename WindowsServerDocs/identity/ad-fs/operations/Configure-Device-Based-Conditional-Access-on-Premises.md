---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: 設定裝置型條件式存取內部部署
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0962012fa0fc6190df2b0dccdd63664cd5264800
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954390"
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>使用已註冊的裝置設定內部部署條件式存取


下列檔將引導您安裝和設定使用已註冊裝置的內部部署條件式存取。

![條件式存取](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>基礎結構先決條件
在您開始使用內部部署條件式存取之前，必須先執行下列每個元件。 

|需求|描述
|-----|-----
|具有 Azure AD Premium 的 Azure AD 訂用帳戶 | 啟用內部部署條件式存取的裝置回寫-[免費試用版正常](https://azure.microsoft.com/trial/get-started-active-directory/)  
|Intune 訂閱|只有適用于裝置合規性案例的 MDM 整合才需要-[免費試用版正常](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|2015年11月 QFE 或更新版本。  在[這裡](https://www.microsoft.com/download/details.aspx?id=47594)取得最新版本。  
|Windows Server 2016|適用于 AD FS 的組建10586或更新版本  
|Windows Server 2016 Active Directory 架構|需要架構層級85或更高版本。
|Windows Server 2016 網域控制站|這只是 Hello 企業版金鑰信任部署的必要條件。  您可以在[這裡](https://aka.ms/whfbdocs)找到其他資訊。  
|Windows 10 用戶端|只有加入上述網域的組建10586或更新版本，才能加入 Windows 10 網域和 Microsoft Passport for Work 案例  
|已指派 Azure AD Premium 授權的 Azure AD 使用者帳戶|註冊裝置  


 
## <a name="upgrade-your-active-directory-schema"></a>升級您的 Active Directory 架構
若要將內部部署條件式存取與已註冊的裝置搭配使用，您必須先升級您的 AD 架構。  必須符合下列條件：
    - 架構應為85或更新版本
    - 只有 AD FS 加入的樹系才需要此參數

> [!NOTE]
> 如果您在升級至 Windows Server 2016 中的架構版本（層級85或更高）之前安裝 Azure AD Connect，您必須重新執行 Azure AD Connect 安裝，並重新整理內部部署 AD 架構，以確保已設定 Msds-keycredentiallink 的同步處理規則。

### <a name="verify-your-schema-level"></a>驗證您的架構層級
若要驗證您的架構層級，請執行下列動作：

1.  您可以使用 ADSIEdit 或 LDP，並連接到架構命名內容。  
2.  使用 ADSIEdit，以滑鼠右鍵按一下 [CN = Schema，CN = Configuration，DC = <domain> ，dc =]， <com> 然後選取 [屬性]。  Relpace 網域和 com 部分與您的樹系資訊。
3.  在 [屬性編輯器] 下找到 objectVersion 屬性，它會告訴您您的版本。  

![ADSI 編輯器](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

您也可以使用下列 PowerShell Cmdlet （將物件取代為您的架構命名內容資訊）：

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

如需有關升級的詳細資訊，請參閱[將網域控制站升級至 Windows Server 2016](../../ad-ds/deploy/upgrade-domain-controllers.md)。 

## <a name="enable-azure-ad-device-registration"></a>啟用 Azure AD 裝置註冊  
若要設定此案例，您必須在 Azure AD 中設定裝置註冊功能。  

若要這麼做，請遵循[在組織中設定 Azure AD 聯結](/azure/active-directory/devices/device-management-azure-portal)底下的步驟  

## <a name="setup-ad-fs"></a>安裝 AD FS  
1. 建立新的[AD FS 2016 伺服器](../deployment/deploying-a-federation-server-farm.md)陣列。   
2.  或從 AD FS 2012 R2 將伺服器陣列[遷移](../deployment/upgrading-to-ad-fs-in-windows-server.md)至 AD FS 2016  
4. 使用自訂路徑來部署[Azure AD Connect](../deployment/upgrading-to-ad-fs-in-windows-server.md) ，以將 AD FS 連接到 Azure AD。  

## <a name="configure-device-write-back-and-device-authentication"></a>設定裝置回寫和裝置驗證  
> [!NOTE]
> 如果您使用 [快速設定] 執行 Azure AD Connect，就會為您建立正確的 AD 物件。  不過，在大部分 AD FS 案例中，Azure AD Connect 是以自訂設定來執行，以設定 AD FS，因此需要下列步驟。  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>建立 AD FS 裝置驗證的 AD 物件  
如果 AD FS 伺服器陣列尚未進行裝置驗證設定 (您可以在 AD FS 管理主控台的 [服務]-> [裝置註冊] 底下查看是否已設定)，請使用下列步驟建立正確的 AD DS 物件和設定。  

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>注意：下列命令需要 Active Directory 的系統管理工具，因此，如果您的同盟伺服器也不是網域控制站，請先使用下列步驟1安裝工具。  否則略過步驟 1。  

1.  執行 **\[新增角色及功能\]** 精靈，然後選取 **\[遠端伺服器管理工具\]** -> **\[角色管理工具\]** -> **\[AD DS 及 AD LDS 工具\]** -> 選擇 **\[Windows PowerShell 的 Active Directory 模組\]** 及 **\[AD DS 工具\]**。

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2. 在您的 AD FS 主伺服器上，確定您是以具有企業系統管理員（EA）許可權 AD DS 使用者的身分登入，並開啟已提升許可權的 powershell 提示字元。  然後，執行下列 PowerShell 命令：  
    
   `Import-module activedirectory`  
   `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3. 在快顯視窗上，按 [是]。

>注意：如果您的 AD FS 服務設定為使用 GMSA 帳戶，請以 "domain\accountname $" 格式輸入帳戶名稱

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

上述 PSH 會建立下列物件：  


- AD 網域磁碟分割區中的 RegisteredDevices 容器  
- [設定] --> [服務] --> [裝置註冊設定] 下的裝置註冊服務容器及物件  
- [設定] --> [服務] --> [裝置註冊設定] 下的裝置註冊服務 DKM 容器及物件  

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4. 完成此作業後，您就會看到成功完成訊息。

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>在 AD 中建立服務連接點（SCP）  
如果您打算使用此處所述的 Windows 10 網域加入 (搭配 Azure AD 自動註冊)，請執行下列命令以建立 AD DS 中的服務連接點  
1.  開啟 Windows PowerShell，然後執行下列命令：
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>注意：如有必要，請從您的 Azure AD Connect 伺服器複製 AdSyncPrep .psm1 檔案。  此檔案位於 Program Files\Microsoft Azure Active Directory Connect\AdPrep

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. 提供您的 Azure AD 全域管理員認證  

    `PS C:>$aadAdminCred = Get-Credential`

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3. 執行下列 PowerShell 命令 

   `PS C:>Initialize-ADSyncDomainJoinedComputerSync -AdConnectorAccount [AD connector account name] -AzureADCredentials $aadAdminCred ` 

其中 [AD 連接器帳戶名稱] 是您新增內部部署 AD DS 目錄時，在 Azure AD Connect 中設定的帳戶名稱。
  
上述命令可讓 Windows 10 用戶端在 AD DS 中建立 serviceConnectionpoint 物件，以尋找要加入的正確 Azure AD 網域。  

### <a name="prepare-ad-for-device-write-back"></a>準備 AD 以進行裝置回寫   
為了確保 AD DS 物件及容器處於可讓裝置從 Azure AD 回寫的正確狀態，請執行下列動作。

1.  開啟 Windows PowerShell，然後執行下列命令：  

    `PS C:>Initialize-ADSyncDeviceWriteBack -DomainName <AD DS domain name> -AdConnectorAccount [AD connector account name] ` 

其中 [AD 連接器帳戶名稱] 是您新增內部部署 AD DS 目錄時，在 Azure AD Connect 中設定的帳戶名稱，格式如 domain\accountname。  

上述命令會建立下列供裝置回寫至 AD DS 的物件 (如果還不存在這些物件)，並允許存取指定的 AD 連接器帳戶名稱  

- AD 網域磁碟分割區中的 RegisteredDevices 容器  
- [設定] --> [服務] --> [裝置註冊設定] 下的裝置註冊服務容器及物件  

### <a name="enable-device-write-back-in-azure-ad-connect"></a>在 Azure AD Connect 中啟用裝置回寫  
如果您之前沒有這麼做，請在 Azure AD Connect 中啟用裝置回寫，方法是再次執行精靈並選取 **\[自訂同步處理選項\]** 設定，然後勾選裝置回寫的方塊並選取您執行了上述 Cmdlet 所在的樹系。  

### <a name="configure-device-authentication-in-ad-fs"></a>在 AD FS 中設定裝置驗證  
使用提升權限的 PowerShell 命令視窗，執行下列命令以設定 AD FS 原則  

`PS C:>Set-AdfsGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true -DeviceAuthenticationMethod All` 

### <a name="check-your-configuration"></a>檢查設定  
以下是裝置回寫及驗證正常運作所需之 AD DS 裝置、容器與權限的完整清單，供您參考
 


- 位於 CN=RegisteredDevices,DC=&lt;domain&gt; 且類型為 ms-DS-DeviceContainer 的物件          
    - AD FS 服務帳戶讀取存取權   
    - Azure AD Connect 同步 AD 連接器帳戶讀寫存取權限</br></br>

- Container CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=&lt;domain&gt;  
- 上述容器中的容器裝置註冊服務 DKM

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device8.png) 
 


- 位於 CN=&lt;guid&gt;,CN=Device Registration

- Configuration,CN=Services,CN=Configuration,DC=&lt;domain&gt; 且類型為 serviceConnectionpoint 的物件  
  - 新物件上指定之 AD 連接器帳戶名稱的讀寫存取權限</br></br> 


- CN = Device Registration Services，CN = Device Registration Configuration，cn = Services，CN = Configuration，DC =&> ltdomain 的 DeviceRegistrationServiceContainer 類型物件  


- 上述容器中類型為 msDS-DeviceRegistrationService 的物件  

### <a name="see-it-work"></a>查看其工作  
若要評估新的宣告和原則，請先註冊裝置。  例如，您可以使用 [系統 > 關於] 底下的 [設定] 應用程式 Azure AD 加入 Windows 10 電腦，或者可以遵循[這裡](/azure/active-directory/devices/hybrid-azuread-join-plan)的其他步驟，使用自動裝置註冊設定 windows 10 網域加入。  如需有關加入 Windows 10 行動裝置版的資訊，請參閱[這裡](/windows/client-management/join-windows-10-mobile-to-azure-active-directory)的檔。  

若要進行最簡單的評估，請使用顯示宣告清單的測試應用程式登入 AD FS。 您將能夠看到新的宣告，包括 isManaged、isCompliant 和 trusttype 內容。  如果您啟用 Microsoft Passport for work，您也會看到 prt 宣告。  
 

## <a name="configure-additional-scenarios"></a>設定其他案例  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>自動註冊加入網域的 Windows 10 電腦  
若要為已加入網域的 Windows 10 電腦啟用自動註冊裝置，請遵循[這裡](/azure/active-directory/devices/hybrid-azuread-join-plan)的步驟1和2。   
這可協助您達成下列目標：  

1. 請確定您 AD DS 中的服務連接點存在，而且具有適當的許可權（我們已在上面建立此物件，但不會傷害重複檢查）。  
2. 確定已正確設定 AD FS  
3. 請確定您的 AD FS 系統已啟用正確的端點，並已設定宣告規則   
4. 為已加入網域的電腦設定自動裝置註冊所需的群組原則設定   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport for Work   
如需使用 Microsoft Passport for Work 啟用 Windows 10 的詳細資訊，請參閱[在您的組織中啟用 Microsoft Passport for Work。](/windows/security/identity-protection/hello-for-business/hello-identity-verification)  

### <a name="automatic-mdm-enrollment"></a>自動 MDM 註冊   
若要啟用已註冊裝置的自動 MDM 註冊，讓您可以在存取控制原則中使用 isCompliant 宣告，請遵循[此處](/windows/client-management/join-windows-10-mobile-to-azure-active-directory)的步驟。  

## <a name="troubleshooting"></a>疑難排解  
1.  如果您在該抱怨上收到錯誤 `Initialize-ADDeviceRegistration` ，指出已存在錯誤狀態的物件，例如「找不到具有所有必要屬性的 drs 服務物件」，您可能已在先前執行 Azure AD Connect powershell 命令，並在 AD DS 中具有部分設定。  請嘗試手動刪除 [ **cn = Device Registration configuration，cn = Services，cn = configuration，DC = &lt; domain &gt; ** ] 底下的物件，然後再試一次。  
2.  適用于已加入網域的 Windows 10 用戶端  
    1. 若要確認裝置驗證是否正常運作，請以測試使用者帳戶身分登入加入網域的用戶端。 若要快速觸發布建，請至少一次鎖定並解除鎖定桌面。   
    2. 在 AD DS 物件上檢查 stk 金鑰認證連結的指示（同步處理是否仍然需要執行兩次？）  
3.  如果您在嘗試註冊已註冊裝置的 Windows 電腦時發生錯誤，但無法或尚未取消註冊裝置，您可能會在登錄中擁有裝置註冊設定的片段。  若要調查並移除此項，請使用下列步驟：  
    1. 在 Windows 電腦上，開啟 Regedit 並流覽至**HKLM\Software\Microsoft\Enrollments**   
    2. 在此機碼下，GUID 表單中將會有許多子機碼。  流覽至具有 ~ 17 值的子機碼，並具有 "6" [已加入 MDM] 或 [13] （Azure AD 聯結）的 "EnrollmentType"  
    3. 將**EnrollmentType**修改為**0** 
    4. 再次嘗試註冊裝置或註冊  

### <a name="related-articles"></a>相關文章  
* [保護對 Office 365 及其他連接至 Azure Active Directory 之應用程式的存取](/azure/active-directory/conditional-access/overview)  
* [Office 365 服務的條件式存取裝置原則](/azure/active-directory/conditional-access/overview)  
* [使用 Azure Active Directory 裝置註冊設定內部部署條件式存取](/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [將已加入網域裝置連接到 Azure AD 以體驗 Windows 10](/azure/active-directory/devices/hybrid-azuread-join-plan)  
