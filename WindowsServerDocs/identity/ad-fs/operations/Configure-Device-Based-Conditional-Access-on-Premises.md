---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: "設定裝置為基礎的條件存取先"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 47afd0c6963bd8b8b4dde82650cf807c1954b40b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>使用裝置的且已的設定先條件存取

>適用於：Windows Server 2016、Windows Server 2012 R2  

下列文件將引導您進行安裝及的裝置且已設定條件場所上的存取。

![條件存取](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>基礎結構必要條件
您可以先條件存取開始下列每一位必要條件所需的。 

|需求|描述
|-----|-----
|使用 Azure AD Premium Azure AD 裝機費 | 若要讓裝置寫入回復為前提條件存取-在[免費試用版很好](https://azure.microsoft.com/en-us/trial/get-started-active-directory/)  
|Intune 裝機費|只需 MDM 整合裝置 compliance 案例-[免費試用版很好](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD 連接|年 11 月 2015 QFE 或更新版本。  取得最新的版本[在此](https://www.microsoft.com/en-us/download/details.aspx?id=47594)。  
|Windows Server 2016|組建 10586 或較新的 AD FS  
|Windows Server 2016 Active Directory 架構|需要架構層級 85 或更高版本。
|Windows Server 2016 網域控制站|這只是必要的 Hello 適用於企業鍵信任部署。  在找到的其他資訊[在此](https://aka.ms/whfbdocs)。  
|Windows 10 client|組建 10586，或在較新、 連接到上述網域工作案例僅適用於 Windows 10 加入網域和 Microsoft Passport 需要  
|使用 Azure AD Premium 授權指派 azure AD 帳號|適用於登記裝置  


 
## <a name="upgrade-your-active-directory-schema"></a>升級您的 Active Directory 結構描述
為了使用先條件存取的且已裝置時，您必須先升級您的廣告結構描述。  必須符合下列條件：
    - 架構應該 85 或更高版本
    - 這只是必要的樹系 AD FS 加入

> [!NOTE]
> 如果您安裝 Azure AD 連接之前升級到 Windows Server 2016 中的架構版本（層級 85 或大），您將需要重新執行 Azure AD 連接安裝並重新整理上場所 AD 架構 msDS-KeyCredentialLink 設定為確保同步規則。

### <a name="verify-your-schema-level"></a>確認您的架構層級
若要確認您的架構層級，請執行下列動作：

1.  您可以使用 ADSIEdit 或 LDP，並連接到架構命名操作。  
2.  使用 ADSIEdit，以滑鼠右鍵按一下「DATA-CN = 區結構描述 DATA-CN = 設定，俠 =<domain>，DC =<com>，然後選取 [屬性。  Relpace 網域和 com 部分的樹系資訊。
3.  在屬性編輯器尋找係屬性，它將會通知您，您的版本。  

![編輯 ADSI](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

您也可以使用下列 PowerShell cmdlet（取代您架構命名操作資訊物件）：

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

為升級的詳細資訊，請查看[升級到 Windows Server 2016 的網域控制站](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md)。 

## <a name="enable-azure-ad-device-registration"></a>讓登記 Azure AD 的裝置  
若要設定此案例，您必須設定 Azure AD 的裝置登記功能。  

若要這樣做，請依照下[設定在組織中 Azure AD Join](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>AD FS 設定  
1. 建立[新 AD FS 2016 發電廠](https://technet.microsoft.com/library/dn486775.aspx)。   
2.  或者[移轉](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)ad FS 2016 發電廠從 AD FS 2012 R2  
4. 部署[Azure AD 連接](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/)連接 Azure AD AD FS 使用的自訂路徑。  

## <a name="configure-device-write-back-and-device-authentication"></a>設定裝置寫入回和裝置驗證  
> [!NOTE]
> 如果您在使用快速設定 Azure AD 連接，已為您建立正確 AD 物件。  但是，在大部分案例中 AD FS，Azure AD 連接執行自訂設定] 來設定，AD FS 使用，以下步驟會需要。  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>AD FS 裝置驗證建立廣告物件  
如果您 AD FS 發電廠不已設定為裝置驗證 （您可以看到這個在 AD FS 管理主控台中，在 [服務]-> [裝置登記），請使用下列步驟來建立正確 AD DS 物件和設定。  

![裝置登記](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>注意： 下列命令需要 Active Directory 系統管理工具，因此如果您聯盟伺服器也不是網域控制站，第一次安裝的工具，使用下列步驟 1。  或者跳過步驟 1。  

1.  執行**新增角色與功能**功能精靈，並選取**遠端伺服器管理工具** -> **角色管理工具** -> **AD DS 與廣告 LDS 工具**這兩個選擇]-> [ **Active Directory Windows PowerShell 模組**和**AD DS 工具**。

![裝置登記](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2.  AD FS 主要伺服器，請確定您以企業系統管理員 (EA) 權限 AD DS 使用者登入並開放提升權限的 powershell 命令提示字元。  然後執行下列命令 PowerShell:  
    
    `Import-module activedirectory`  
    `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3.  在快顯視窗按下 [是]。

>注意： 如果您 AD FS 服務設定要使用 GMSA 帳號，account 中輸入名稱的格式 」 domain\accountname$ 」

![裝置登記](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

上述 PSH 建立下列物件：  


- 在 AD 網域磁碟分割 RegisteredDevices 容器  
- 裝置登記服務容器和設定中的物件--> 服務--> [裝置登記設定  
- 裝置登記服務 DKM 容器和設定中的物件--> 服務--> [裝置登記設定  

![裝置登記](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4.  完成之後，您會看到完成成功的訊息。

![裝置登記](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>建立廣告服務連接點 (SCP)  
如果您打算所述以下使用 Windows 10 （使用自動的登記 Azure ad） 加入網域，，執行下列命令來建立服務連接點 AD DS  
1.  打開 Windows PowerShell，執行下列動作：
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>注意： 若有需要，AdSyncPrep.psm1 將檔案複製從您的 Azure AD 連接伺服器。  此檔案位於程式必要 Azure Active Directory Connect\AdPrep

![裝置登記](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. 提供您 Azure AD 的全域管理員認證  

    `PS C:>$aadAdminCred = Get-Credential`

![裝置登記](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3.  執行下列 PowerShell 命令 

    `PS C:>Initialize-ADSyncDomainJoinedComputerSync -AdConnectorAccount [AD connector account name] -AzureADCredentials $aadAdminCred ` 

廣告連接器 account 姓名所在帳號，您設定 Azure AD 連接時，將您先新增名稱 AD DS directory。
  
上述的命令可讓 Windows 10 戶端尋找正確建立 serviceConnectionpoint 物件 AD DS 在加入 Azure AD 網域。  

### <a name="prepare-ad-for-device-write-back"></a>回到裝置寫入準備廣告   
若要確保 AD DS 物件與容器寫入背面 Azure AD 的裝置是正確的狀態，請執行下列項目。

1.  打開 Windows PowerShell，執行下列動作：  

    `PS C:>Initialize-ADSyncDeviceWriteBack -DomainName <AD DS domain name> -AdConnectorAccount [AD connector account name] ` 

廣告連接器 account 姓名所在帳號，您設定 Azure AD 連接時，將您先新增名稱 AD DS directory domain\accountname 格式  

建立下列物件裝置寫入到 AD DS，它們不存在，如果上述命令，並允許存取指定 AD 連接器 account 名稱  

- AD 網域磁碟分割中 RegisteredDevices 容器  
- 裝置登記服務容器和設定中的物件--> 服務--> [裝置登記設定  

### <a name="enable-device-write-back-in-azure-ad-connect"></a>讓裝置寫入入 Azure AD 連接  
如果您進行之前，請讓裝置寫入入 Azure AD 連接滑鼠第二次執行精靈，並選取 [ **[自訂同步選項]**，然後回裝置寫的核取方塊，然後選取您在其中執行上述 cmdlet 樹系  

### <a name="configure-device-authentication-in-ad-fs"></a>設定裝置驗證，AD FS 中  
使用較高的 PowerShell 命令視窗中，執行下列命令設定 AD FS 原則  

`PS C:>Set-AdfsGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true -DeviceAuthenticationMethod All` 

### <a name="check-your-configuration"></a>檢查您的設定  
供您參考，以下是 AD DS 裝置、 容器和運作所需裝置回寫與驗證的權限的完整清單
 


- 物件的類型 ms-DS-DeviceContainer 在 DATA-CN = RegisteredDevices 特區 =&lt;網域&gt;        
    - AD FS 服務 account 讀取權限   
    - 讀取/寫入 Azure AD 連接同步 AD 連接器 account</br></br>

- 容器 DATA-CN = 裝置登記組態 DATA-CN = 服務 DATA-CN = 設定，俠 =&lt;網域&gt;  
- 容器裝置登記服務 DKM 在上面容器

![裝置登記](media/Configure-Device-Based-Conditional-Access-on-Premises/device8.png) 
 


- 物件的類型 serviceConnectionpoint 在 DATA-CN =&lt;guid&gt;，DATA-CN = 裝置登記

- 設定、 DATA-CN = 服務 DATA-CN = 設定，俠 =&lt;網域&gt;  
 - 讀取/寫入存取指定新物件 AD 連接器 account 名稱</br></br> 


- 物件的類型 msDS-DeviceRegistrationServiceContainer 在 DATA-CN = 裝置登記服務 DATA-CN = 裝置登記組態 DATA-CN = 服務 DATA-CN = 設定，俠 = 與 ltdomain >  


- 輸入 msDS-DeviceRegistrationService 上述容器中的物件  

### <a name="see-it-work"></a>查看工作  
評估新宣告和原則，第一次登記裝置。  例如，您可以 Azure AD Join Windows 10 的電腦使用 「 設定 」 app 在 [系統]-> [關於，或您可以設定 Windows 10 加入網域自動裝置登記下列額外步驟以[在此](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)。  有關加入 Windows 10 行動裝置版裝置時，會看到文件[在此](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)。  

適用於簡單評估版，登入 AD FS 使用的測試應用程式，會顯示索賠項目清單。 您將會看到新宣告包括 isManaged、 isCompliant，以及 trusttype。  如果您的工作讓 Microsoft Passport，您也會看到 prt 取得。  
 

## <a name="configure-additional-scenarios"></a>設定其他案例  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>自動登記適用於 Windows 10 加入網域的電腦  
若要讓 Windows 10 網域自動裝置登記加入電腦，請依照下列步驟 1 到 2[在此](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)。   
這將會幫助您達到下列動作：  

1. 確定您的服務連接點 AD ds 存在，且具有的適當權限 （我們建立了這個物件上述，但它不會傷害點檢查）。  
2. 確定已正確 AD FS  
3. 確保您的系統 AD FS 已正確結束支援，以及取得設定規則   
4. 設定所需的自動裝置登記加入網域的電腦的群組原則設定   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport 工作   
讓工作 Microsoft Passport 與 Windows 10 上的資訊，請查看[您在組織中工作讓 Microsoft Passport。](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>自動 MDM 註冊   
若要自動 MDM 註冊的且已裝置，您可以使用您存取控制原則 isCompliant 宣告，請依照下列步驟執行[在此。](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>疑難排解  
1.  如果您收到錯誤，`Initialize-ADDeviceRegistration`的抱怨相關的物件現有錯誤的狀態，例如 「 drs 服務物件找到不需要的所有屬性 」，您執行 Azure AD 連接 powershell 命令先前和 AD DS 中有部分的設定。  請嘗試手動刪除下的物件**DATA-CN = 裝置登記組態 DATA-CN = 服務 DATA-CN = 設定，俠 =&lt;網域&gt;** ，然後再試一次。  
2.  適用於 Windows 10 網域加入戶端  
    1. 確認該裝置驗證正常運作，請登入網域結合 client 成為測試使用者 account。 觸發提供快速、 鎖定和解除鎖定桌面至少一次。   
    2. 連結 AD DS 物件上的指示來檢查是否有 stk 金鑰 credential （會同步仍然可以執行兩次？）  
3.  如果您收到錯誤嘗試登記 Windows 電腦已經已退出裝置，但您無法或已經有 unenrolled 裝置時，您可能會有裝置註冊設定的片段登錄中。  檢查並移除此項，請使用下列步驟：  
    1. 在 Windows 電腦上，開放 Regedit 並瀏覽至**HKLM\Software\Microsoft\Enrollments**   
    2. 此機碼，將會有許多子 GUID 表單。  瀏覽至子有 ~ 17 值，並具有 「 EnrollmentType 」 的 「 6 「 [加入 MDM] 或 [13 」 (加入 Azure AD)  
    3. 修改**EnrollmentType**以**0** 
    4. 再試一次裝置註冊或登記  

### <a name="related-articles"></a>相關文件  
* [連接保護存取 Office 365 和其他應用程式 Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access/)  
* [Office 365 服務條件存取裝置原則](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access-device-policies/)  
* [設定使用 Azure Active Directory 裝置登記先條件存取](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [加入網域的裝置連接到 Azure AD 的 Windows 10 的體驗](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  
