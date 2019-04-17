---
title: "建立 Cfg.ini 檔案"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93a73556-22ef-402d-b8d4-582b74c22bcf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 967db5f36ea27fb04eab9a6682a106ba0072d45d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-cfgini-file"></a>建立 Cfg.ini 檔案

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

Cfg.ini 檔案用來自動安裝在下列案例中的作業系統中：  
  
-   當測試的目標電腦上的預先安裝映像的使用者體驗的初始設定區段來引導安裝以手動或自動模式。 若要這樣做，請查看[建立的初始設定區段](Create-the-Cfg.ini-File.md#BKMK_CreateInit2)。  
  
##  <a name="BKMK_CreateInit2"></a>建立的初始設定一節  
 逐步以手動或自動模式安裝 cfg.ini 檔案中使用的初始設定區段。  
  
#### <a name="to-define-the-initial-configuration-section"></a>若要定義的初始設定一節  
  
1.  在「記事本」開放 cfg.ini 檔案，如果有的話;否則，請建立新的檔案。  
  
2.  新增下列文字來建立 InitialConfiguration 區段。  
  
    ```  
  
    [InitialConfiguration]  
    ;Optional, display language can only be one of the installed language  
    Language=en-us  
    ;Optional, The name of a script that runs after setupComplete.cmd but before the initial configuration begins.  
    ;Optional  
    Locale=en-us  
    ;Optional  
    Country=US  
    ;Optional  
    Keyboard=0409:00000409  
    AcceptEula=true  
    ;This is only required on a server where an OEM EULA has been specified   
    ;by using the OOBE.xml file  
    AcceptOEMEula=true  
    ;Optional. Example: My Company Name  
    CompanyName=EnterCompanyName  
    ServerName=EnterServerName  
    ; Example: CONTOSO  
    NetbiosName=EnterNetbiosDomainName  
    ; Example: contoso.local  
    DNSName=EnterDNSDomain   
    ; Used to set the user name for the domain admin  
    UserName=EnterDomainAdminUserName  
    ;The password has to be strong and at least 8 characters  
    PlainTextPassword=EnterAdminPassword  
    ;. Used to set the user name for the domain standard user account. Ignored in migration mode.  
    StdUserName=EnterDomainStandardUserName  
    ;. The password for the domain standard user account has to be strong and at least 8 characters  
    StdUserPlainTextPassword=EnterStandardUserPassword  
    ;Controls the Watson and automatic update settings  
    Settings=All or Updates or None  
    WebDomainName=www.abc.com  
    TrustedCertFileName=c:\cert\a.pfx  
    TrustedCertPassword=Enteryourpassword  
    EnableVPN=true  
    EnableRWA=true  
    IPv4DNSForwarder=<IPV4Address,IPV4Address,¦>  
    IPv6DNSForwarder=<IPV6Address,IPV6Address,¦>  
    VpnIPv4StartAddress=<IPV4Address>  
    VpnIPv4EndAddress=<IPV4Address>  
    VpnBaseIPv6Address=<IPV6Address>  
    VpnIPv6PrefixLength=<number>  
    ;All these section are optional.  
     [PostOSInstall]  
    ;Optional, The name of a script that runs after setupComplete.cmd but before the initial configuration begins.  
  
    IsHosted=true  
    StaticIPv4Address=<IPV4Address>  
    StaticIPv4Gateway=<IPV4Address>  
    StaticIPv4SubnetMask=<IPV4SubnetMask>   
    StaticIPv6Address=<IPV6Address>  
    StaticIPv6SubnetPrefixLength=<number>  
    StaticIPv6Gateway=<IPV6Address>  
    ClientBackupOn=true  
    FileHistoryOn=true  
    LaunchPadHiddenTasks=<Microsoft.LaunchPad.AdminDashboard,Microsoft.LaunchPad.Backup>  
  
    ```  
  
    > [!NOTE]
    >  未提供的選項，在初始設定期間選擇不同的語言。 如果重設系統語言的作業系統會原先一個。  
  
    |參數名稱|參數描述|  
    |--------------------|---------------------------|  
    |*AcceptEula*|表示使用者接受 Microsoft 軟體授權條款。 可以等於 True 或 False，但此設定為 True，才會繼續進行安裝。|  
    |*AcceptOEMEula*|（選擇性）表示使用者接受合作夥伴授權合約。 值可以等於 True 或 False。 只有當伺服器已購買的合作夥伴提供不同授權合約，此欄位會需要。|  
    |*供應商*|（選擇性）在公司名稱。 使用您的公司名稱與您的公司您的伺服器，並以自訂您的公司報告。 可以將最多 254 個字元。|  
    |*國家/地區*|（選擇性）字串，表示您想要的國家/地區。 範例：我們針對美國。|  
    |*伺服器名稱*|伺服器名稱辨識網路上的伺服器。 您的伺服器名稱必須符合下列條件：<br /><br /> -可以將最多的 15 字元。<br /><br /> -能字母、數字和連字號（-）。<br /><br /> -不得以連字號。<br /><br /> -不得包含任何空間。<br /><br /> -必須不只包含數字。<br /><br /> 範例：ContosoServer。|  
    |*DNSName*|內部網域群組伺服器和 client 電腦，才能分享通用資料庫的使用者名稱、密碼，以及其他常見的資訊。 登入他們的電腦，但它使用內部僅並不網際網路網域名稱相同時，使用者會看到這個名稱。 內部網域名稱必須符合的條件相同所指定的*伺服器名稱*。<br /><br /> 範例：contoso.local。|  
    |*NetbiosName*|NetBIOS 名稱用來辨識的伺服器執行的資源。 它可以是最多的 15 字元。 範例︰ 以 Contoso。|  
    |*語言*|（選擇性）指定顯示語言。 它只是其中一個已安裝語言。 範例：en-us-我們為您使用美國英文。|  
    |*地區設定*|（選擇性）使用指定的時間，以及貨幣格式，*LocaleID*格式。 範例：en-us-我們的貨幣和時間以英文顯示並使用美國標準來格式化。|  
    |*鍵盤*|在下列兩個格式可以鍵盤：<br /><br /> - **輸入的語言：鍵盤配置。** 例如 0409:00000409 其中 0409 年之前**:**已輸入的語言，和**00000409**時的鍵盤配置。 您可以找到清單中的鍵盤配置登錄在**HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Keyboard 配置**。<br /><br /> - **輸入語言：的輸入法識別碼。** 以下是識別碼輸入法的完整清單。<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{8F96574E-C86C-4bd6-9666-3F7327D4CBE8} 阿姆哈拉文輸入法<br /><br /> -{81d4e9c9-1d3b-41bc-9e6c-4b40bf79e35e}{FA550B04-5AD7-411F-A5AC-CA038EC515D7} 微軟拼音-簡單快速頻道（簡體中文）<br /><br /> {531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{B2F9C502-1742-11D4-9790-0080C882687E} 中文（繁體）的-新文注音<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{4BDF9F03-C7D3-11D4-B2AB-0080C882687E} 中文（繁體）-倉頡輸入法<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{6024B45F-5C54-11D4-B921-0080C882687E} 中文（繁體）-快速<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{D38EFF65-AA46-4FD5-91A7-67845FB02F5B} 中文傳統陣列<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{037B2C25-480C-4D7F-B027-D6CA6B69788A} 中文傳統大易<br /><br /> -{03B5835F-F03C-411B-9CE2-AA23E1171E36}{A76C93D9-5523-4E90-AAFA-4DB112F9AC76} Microsoft 輸入法（日本）<br /><br /> -{A028AE76-01B1-46C2-99C4-ACD9858AE02F}{B5FE1F02-D5F2-4445-9C03-C568F23C99A1} Microsoft 輸入法（韓國）<br /><br /> -{A1E2B86B-924A-4D43-80F6-8A820DF7190F}{B60AF051-257A-46BC-B9D3-84DAD819BAFB} 舊韓文輸入法（韓國）<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{409C8376-007B-4357-AE8E-26316EE3FB0D} Yi 輸入法<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{3CAB88B7-CC3E-46A6-9765-B772AD7761FF} Tigrinya 輸入法|  
    |*設定*|設定使用者更新的選擇。 使用其中一項下列值：<br /><br /> **-所有**等於使用建議的設定。<br /><br /> **-更新**等安裝重要的更新。 僅限<br /><br /> **-無**等不要檢查更新。|  
    |*使用者名稱*|在設定期間建立新的系統管理員 account-名稱。 您的系統管理員身分和標準使用者 account 名稱必須符合下列條件：<br /><br /> -可以將最多 19 字元。<br /><br /> -不包含日 \ [] 與 #124;< > + =;, ? *<br /><br /> -必須開頭或結尾的一段時間。<br /><br /> -不得包含兩個句點。<br /><br /> -不能相同的伺服器名稱或內部網域名稱。<br /><br /> -不得預先定義的使用者名稱，例如系統管理員或客體相同。|  
    |*PlainTextPassword*|這是在設定期間建立新的系統管理員 account 的密碼。<br /><br /> -必須至少為八個字元。<br /><br /> -必須包含至少三種四個下列類型：<br /><br /> -大寫字元。<br /><br /> -小寫的字元。<br /><br /> -數字。<br /><br /> -符號。|  
    |*StdUserName*|新的標準使用者 account 在設定期間所建立的名稱。 查看*的使用者名稱*參數的需求。|  
    |*StdUserPlainTextPassword*|標準使用者帳號，會建立在設定期間的密碼。|  
    |WebDomainName|（選擇性）設定網際網路 server 的完整網域名稱。 此檔案可讓您設定的網域名稱安裝精靈中手動設定使用的方法類似的網域名稱。|  
    |TrustedCertFileName|（選擇性）設定的網域名稱受信任的憑證。 這可讓您將。PFX 憑證，其中包含私密金鑰。|  
    |TrustedCertPassword|（選擇性）匯入的密碼。PFX。|  
    |EnableVPN|（選擇性）預設關閉的 VPN。|  
    |VpnIPv4StartAddress|（選擇性）設定 VPN 開始地址。|  
    |VpnIPv4EndAddress|（選擇性）設定 VPN 結束地址。|  
    |VpnBaseIPv6Address|（選擇性）設定 VPN 基礎 IPV6 位址。|  
    |VpnIPv6PrefixLength|（選擇性）設定的 VPN IPv6 位址首碼長度。|  
    |IsHosted|（選擇性）預設值是 false 未指定。 如果您設定這項環境中，設定此值。 它會關閉路由器設定。|  
    |StaticIPv4Address|（選擇性）如果您想要設定的靜態 IP 位址，而不是動態位址，指定靜態 IP 位址。|  
    |StaticIPv4Gateway|（選擇性）如果您想要設定的靜態 IP 位址，而不是動態位址，指定預設閘道位址。|  
    |StaticIPv4SubnetMask|（選擇性）指定子網路遮罩，如果您想要設定的靜態 IP 位址，而不是動態位址。|  
    |StaticIPv6Address|（選擇性）如果您想要設定的靜態 IP 位址，而不是動態位址，指定預設 IP 位址。|  
    |StaticIPv6SubnetPrefixLength|（選擇性）指定 IPV6 子網路首碼長度預設，如果您想要設定的靜態 IP 位址，而不是動態位址。|  
    |StaticIPv6Gateway|（選擇性）如果您想要設定的靜態 IP 位址，而不是一份動態，指定預設閘道位址。|  
    |ClientBackupOn|（選擇性）關閉 Client 備份預設戶端新加入的伺服器。|  
    |FileHistoryOn|（選擇性）關閉檔案歷史備份預設新戶端執行 Windows 8 Consumer Preview 加入伺服器。|  
    |EnableRWA|它可以讓遠端 Web 存取安裝 Windows Server Essentials，但將會跳過路由器設定。 這只被支援 product 的全新安裝。 預設值是 false。|  
    |IPv4DNSForwarder|設定 IPv4 DNS 轉寄。|  
    |IPv6DNSForwarder|設定 IPv6 DNS 轉寄。|  
    |LaunchPadHiddenTasks|-（選擇性）您可以隱藏備份的項目或日和系統管理員儀表板上 Launchpad 的項目。<br /><br /> -來停用儀表板：LaunchPadHiddenTasks=Microsoft.LaunchPad.AdminDashboard<br /><br /> -來停用備份：LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup<br /><br /> -來停用備份與儀表板：LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup,Microsoft.LaunchPad.AdminDashboard|  
  
3.  儲存檔案。 請確定您將檔案儲存為 cfg.ini，cfg.ini.txt。  
  
    > [!NOTE]
    >  您可以將檔案儲存到 USB 快閃磁碟機，可以用於特定階段安裝或 cfg.ini 檔案可能位於根的目標伺服器上的任何硬碟機。 您必須確保檔案編碼設定為 [ANSI 或 Unicode，不支援 UTF-8 編碼。  
  
> [!IMPORTANT]
>  Cfg.ini 的初始設定區段只能伺服器個人化的使用者或的合作夥伴使用自動的回應檔案測試伺服器的使用者體驗。 本節中的檔案不是用來建立映像。  
  
## <a name="see-also"></a>也了  

 [開始使用 Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)

 [開始使用 Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](../install/Additional-Customizations.md)   
 [準備部署映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](../install/Testing-the-Customer-Experience.md)

