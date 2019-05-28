---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: 設定裝置型條件式存取內部部署
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0df290248f049b3f8a823e902cefa860fa074091
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189848"
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>使用已註冊的裝置設定內部部署條件式存取


下列文件將引導您完成安裝和設定內部部署條件式存取與已註冊的裝置。

![條件式存取](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>基礎結構的必要條件
您可以開始使用內部部署條件式存取之前需要下列必要條件。 

|需求|描述
|-----|-----
|使用 Azure AD Premium 的 Azure AD 訂用帳戶 | 若要啟用裝置寫入回在內部部署條件式存取-[是正常的免費試用版](https://azure.microsoft.com/trial/get-started-active-directory/)  
|Intune 訂用帳戶|只有所需的裝置合規性案例-MDM 整合[是正常的免費試用版](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|11 月 2015 QFE 或更新版本。  取得最新版[此處](https://www.microsoft.com/en-us/download/details.aspx?id=47594)。  
|Windows Server 2016|組建 10586 或更新版本，適用於 AD FS  
|Windows Server 2016 的 Active Directory 結構描述|需要結構描述層級 85 部或更高版本。
|Windows Server 2016 網域控制站|這只是為了 Hello 的業務索引鍵信任層級部署。  其他資訊位於[此處](https://aka.ms/whfbdocs)。  
|Windows 10 用戶端|組建 10586 或更新版本，加入上述的網域和所需的 Windows 10 網域加入 Microsoft Passport 只適合工作  
|使用 Azure AD Premium 授權指派的 azure AD 使用者帳戶|註冊裝置  


 
## <a name="upgrade-your-active-directory-schema"></a>升級您的 Active Directory 結構描述
若要使用內部部署條件式存取與已註冊的裝置，您必須先升級您的 AD 結構描述。  必須符合下列條件：
    - 結構描述應為 85 部或更新版本的版本
    - 這只是需要 AD FS 已加入的樹系

> [!NOTE]
> 如果您已安裝 Azure AD Connect，然後才能升級到 Windows Server 2016 中的結構描述版本 （層級 85 部或更高），您必須重新執行 Azure AD Connect 安裝並重新整理內部部署 AD 結構描述，以確保同步處理規則設定 Msds-keycredentiallink。

### <a name="verify-your-schema-level"></a>請確認您的結構描述層級
若要確認您的結構描述層級，執行下列作業：

1.  您可以使用 ADSIEdit 或 LDP，並連接到結構描述命名內容。  
2.  使用 ADSIEdit，以滑鼠右鍵按一下"CN = Schema，CN = Configuration，DC =<domain>，DC =<com>並選取 [屬性]。  Relpace 網域和樹系資訊的 com 部分。
3.  在 [屬性編輯器] 下找出 objectVersion 屬性，然後它會告訴您，您的版本。  

![ADSI 編輯器](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

您也可以使用下列 PowerShell cmdlet （取代您命名內容資訊的結構描述的物件）：

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

如需有關升級的詳細資訊，請參閱[網域控制站升級到 Windows Server 2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md)。 

## <a name="enable-azure-ad-device-registration"></a>啟用 Azure AD 裝置註冊  
若要設定此案例中，您必須在 Azure AD 中設定裝置註冊功能。  

若要這樣做，請遵循下方步驟[設定您組織中的 Azure AD Join](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>設定 AD FS  
1. 建立[新的 AD FS 2016 伺服器陣列](https://technet.microsoft.com/library/dn486775.aspx)。   
2.  或是[移轉](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)從 AD FS 2012 R2 ad FS 2016 伺服器陣列  
4. 部署[Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/)使用自訂路徑來連線至 Azure AD 的 AD FS。  

## <a name="configure-device-write-back-and-device-authentication"></a>設定裝置寫回 」 和 「 裝置驗證  
> [!NOTE]
> 如果您執行使用 Express 設定 Azure AD Connect 時，有已為您建立正確的 AD 物件。  不過，在大部分的 AD FS 案例中，Azure AD Connect 已執行與自訂設定來設定 AD FS 中，因此下列步驟所需。  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>建立 AD FS 裝置驗證的 AD 物件  
如果 AD FS 伺服器陣列尚未進行裝置驗證設定 (您可以在 AD FS 管理主控台的 [服務]-> [裝置註冊] 底下查看是否已設定)，請使用下列步驟建立正確的 AD DS 物件和設定。  

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>注意:以下命令需要 Active Directory 系統管理工具，如果您的同盟伺服器也不是網域控制站，請先利用下面的步驟 1 來安裝工具。  否則略過步驟 1。  

1.  執行 **\[新增角色及功能\]** 精靈，然後選取 **\[遠端伺服器管理工具\]**  ->  **\[角色管理工具\]**  ->  **\[AD DS 及 AD LDS 工具\]** -> 選擇 **\[Windows PowerShell 的 Active Directory 模組\]** 及 **\[AD DS 工具\]** 。

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2.  在您的 AD FS 主要伺服器，請確定您具有企業系統管理員 」 (EA) 的權限的 AD DS 使用者身分登入，並開啟提升權限的 powershell 命令提示字元。  接著，執行下列 PowerShell 命令：  
    
    `Import-module activedirectory`  
    `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3.  在快顯視窗上按 [是]。

>注意:如果 AD FS 服務已設定要使用 GMSA 帳戶，請依格式「domain\accountname$」輸入帳戶名稱

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

上述 PSH 會建立下列物件：  


- AD 網域磁碟分割區中的 RegisteredDevices 容器  
- [設定] --> [服務] --> [裝置註冊設定] 下的裝置註冊服務容器及物件  
- [設定] --> [服務] --> [裝置註冊設定] 下的裝置註冊服務 DKM 容器及物件  

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4.  完成此作業後，您就會看到成功完成訊息。

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>在 AD 中建立服務連接點 (SCP)  
如果您打算使用此處所述的 Windows 10 網域加入 (搭配 Azure AD 自動註冊)，請執行下列命令以建立 AD DS 中的服務連接點  
1.  開啟 Windows PowerShell，然後執行下列命令：
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>附註： 如有必要，請複製您的 Azure AD Connect 伺服器中 AdSyncPrep.psm1 檔案。  此檔案位於 Program Files\Microsoft Azure Active Directory Connect\AdPrep

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. 提供您的 Azure AD 全域管理員認證  

    `PS C:>$aadAdminCred = Get-Credential`

![裝置註冊](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3.  執行下列 PowerShell 命令 

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

### <a name="check-your-configuration"></a>檢查您的設定  
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


- 物件的型別 msDS-DeviceRegistrationServiceContainer 在 CN = Device Registration Services，CN = Device Registration Configuration，CN = Services，CN = Configuration，DC = & ltdomain >  


- 上述容器中類型為 msDS-DeviceRegistrationService 的物件  

### <a name="see-it-work"></a>查看其運作方式  
若要評估的新宣告和原則，請先註冊裝置。  例如，您可以使用系統設定 應用程式的 Windows 10 電腦]-> [關於，Azure AD Join 或進行自動裝置註冊額外的步驟，您可以設定 Windows 10 網域聯結[此處](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)。  如需加入 Windows 10 行動裝置，請參閱文件[此處](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)。  

對於最簡單的評估，登入 AD FS 使用宣告的清單會顯示測試應用程式。 您會看到新的宣告包括 isManaged、 isCompliant 和 trusttype。  如果您啟用 Microsoft Passport for work 時，您也會看到 prt 宣告。  
 

## <a name="configure-additional-scenarios"></a>設定其他案例  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>自動註冊的 Windows 10 已加入網域的電腦  
若要啟用 Windows 10 網域的自動裝置註冊加入的電腦，請遵循步驟 1 和 2[此處](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)。   
這將協助您達到下列目的：  

1. 請確認您的服務連接點，在 AD DS 中存在且具有適當的權限 （我們在建立此物件，但它不會不會進行詳細的檢查會降低）。  
2. 請確定已正確設定 AD FS  
3. 請確定您的 AD FS 系統已正確啟用的端點和宣告規則設定   
4. 設定自動裝置註冊加入網域的電腦所需的群組原則設定   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport for Work   
如需啟用與 Microsoft Passport for Work 的 Windows 10 的資訊，請參閱[啟用 Microsoft Passport for Work 在您的組織。](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>自動執行 MDM 註冊   
若要啟用的已註冊的裝置的自動 MDM 註冊，使您可以使用 isCompliant 宣告在存取控制原則，請遵循[這裡。](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>疑難排解  
1.  如果您收到錯誤`Initialize-ADDeviceRegistration`，會反映物件中已經存在錯誤的狀態，例如"drs 服務物件被發現具有沒有所有必要的屬性 」，您可能會有先前執行 Azure AD Connect powershell 命令和在 AD DS 中的某項部分設定。  請嘗試手動刪除底下的物件**CN = Device Registration Configuration，CN = Services，CN = Configuration，DC =&lt;網域&gt;** ，然後再試。  
2.  針對 Windows 10 已加入網域的用戶端  
    1. 若要確認該裝置驗證運作，以登入網域聯結用戶端的測試使用者帳戶。 觸發程序快速佈建、 鎖定和解除鎖定桌面至少一次。   
    2. 若要檢查 sdk 金鑰認證的指示連結在 AD DS 物件上 （同步仍必須執行兩次？）  
3.  如果您收到錯誤時嘗試註冊已註冊裝置，但您無法或已取消註冊裝置的 Windows 電腦，您可能必須在登錄中的片段的裝置註冊設定。  若要調查，並移除此，使用下列步驟：  
    1. 在 Windows 電腦上，開啟 Regedit，然後瀏覽至**HKLM\Software\Microsoft\Enrollments**   
    2. 此機碼，會有許多子機碼 GUID 格式。  瀏覽至的子機碼中有大約 17 值，且其為"6"[聯結的 MDM] 的 「 EnrollmentType"或"13"(已加入 Azure AD)  
    3. 修改**EnrollmentType**到**0** 
    4. 重試註冊的裝置註冊  

### <a name="related-articles"></a>相關文章  
* [保護存取 Office 365 和其他應用程式連接至 Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)  
* [Office 365 服務的條件式存取裝置原則](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access-device-policies/)  
* [設定內部部署條件式存取使用 Azure Active Directory 裝置註冊](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [已加入網域的裝置連接到 Azure AD 以體驗 Windows 10](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  
