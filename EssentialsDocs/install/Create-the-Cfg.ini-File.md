---
title: 建立 Cfg.ini 檔案
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820119"
---
# <a name="create-the-cfgini-file"></a>建立 Cfg.ini 檔案

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

您可以在下列情況中使用 cfg.ini 檔案來自動安裝作業系統：  
  
-   在目標電腦上使用預先安裝的映像來測試使用者的經驗時，可使用初始設定區段以自動或非自動安裝模式，逐步完成安裝作業。 若要執行此動作，請參閱[建立初始設定區段](Create-the-Cfg.ini-File.md#BKMK_CreateInit2)。  
  
##  <a name="BKMK_CreateInit2"></a> 建立初始設定區段  
 cfg.ini 檔案中的初始設定區段，可用來以自動或非自動安裝模式，逐步完成安裝作業。  
  
#### <a name="to-define-the-initial-configuration-section"></a>若要定義初始設定區段  
  
1.  在記事本中開啟 cfg.ini 檔案 (如果存在的話)，否則請建立新的檔案。  
  
2.  新增以下文字以建立初始設定區段。  
  
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
    >  在初始設定期間無法提供選取不同語言的選項。 如果重設系統，則作業系統的語言會是原始安裝的語言。  
  
    |參數名稱|參數描述|  
    |--------------------|---------------------------|  
    |*AcceptEula*|指出使用者接受 Microsoft 軟體授權合約。 此值可為 True 或 False，但只有在設為 True 時，安裝才能繼續。|  
    |*AcceptOEMEula*|(選擇性) 指出使用者接受合作夥伴授權合約。 此值可為 True 或 False。 只有向提供個別授權合約的合作夥伴購買伺服器時，才需填入此欄位。|  
    |*CompanyName*|(選用) 公司的名稱。 您的公司名稱是用來將您的伺服器與公司建立關聯，並自訂您的公司報告。 長度最多可達 254 個字元。|  
    |*國家/地區*|(選用) 字串表示要選擇的國家/地區。 範例：US 代表美國。|  
    |*ServerName*|伺服器名稱可以在網路上唯一識別您的伺服器。 您的伺服器名稱必須符合下列準則：<br /><br /> -可以是最多 15 個字元。<br /><br /> -可以包含字母、 數字和連字號 （-）。<br /><br /> -不得以連字號開頭。<br /><br /> -不能包含任何空格。<br /><br /> -必須不只能包含數字。<br /><br /> 範例：ContosoServer。|  
    |*DNSName*|內部網域可將伺服器與用戶端電腦群組在一起，以共用內含使用者名稱、密碼和其他通用資訊的通用資料庫。 使用者在登入電腦時會看見此名稱，但此名稱僅供內部使用，而且與網際網路的網域名稱不同。 內部網域名稱必須符合針對 *ServerName* 指定的相同準則。<br /><br /> 例如：contoso.local。|  
    |*NetbiosName*|NetBIOS 名稱可用來識別伺服器上所執行的資源。 其長度最多可達 15 個字元。 範例：Contoso。|  
    |*語言*|(選擇性) 指定顯示名稱。 這必須是已安裝的語言之一。 例如：en-us 代表美國使用的英文。|  
    |*地區設定*|(選擇性) 使用 *LocaleID* 格式指定時間與貨幣格式。 例如：en-us 代表以英文顯示，並根據美國的使用標準設定格式的貨幣與時間。|  
    |*鍵盤*|鍵盤可以是下列兩種格式之一：<br /><br /> - **輸入的語言： 鍵盤配置。** 例如 0409:00000409，其中 **:** 之前的 0409 是輸入語言，而 **00000409** 是鍵盤配置。 您可以在 **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Keyboard Layouts** 登錄機碼下找到鍵盤配置清單。<br /><br /> - **輸入語言： IME 識別碼。** 以下是 IME 識別碼的完整清單。<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{8F96574E-C86C-4bd6-9666-3F7327D4CBE8} 阿姆哈拉文輸入方法<br /><br /> -Microsoft {81d4e9c9-1d3b-41bc-9e6c-4b40bf79e35e}{FA550B04-5AD7-411F-A5AC-CA038EC515D7} 拼音-簡單的快速 （簡體中文）<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{B2F9C502-1742-11D4-9790-0080C882687E} 中文 （繁體）-新的語音<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{4BDF9F03-C7D3-11D4-B2AB-0080C882687E} 中文 （繁體）-倉頡<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{6024B45F-5C54-11D4-B921-0080C882687E} 中文 （繁體）-快速<br /><br /> - {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{D38EFF65-AA46-4FD5-91A7-67845FB02F5B}            Chinese Traditional Array<br /><br /> - {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{037B2C25-480C-4D7F-B027-D6CA6B69788A}            Chinese Traditional DaYi<br /><br /> - {03B5835F-F03C-411B-9CE2-AA23E1171E36}{A76C93D9-5523-4E90-AAFA-4DB112F9AC76}            Microsoft IME (Japanese)<br /><br /> - {A028AE76-01B1-46C2-99C4-ACD9858AE02F}{B5FE1F02-D5F2-4445-9C03-C568F23C99A1}             Microsoft IME (Korean)<br /><br /> -{A1E2B86B-924A-4D43-80F6-8A820DF7190F}{B60AF051-257A-46BC-B9D3-84DAD819BAFB} 舊韓文輸入法 （韓文）<br /><br /> - {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{409C8376-007B-4357-AE8E-26316EE3FB0D}              Yi Input Method<br /><br /> - {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{3CAB88B7-CC3E-46A6-9765-B772AD7761FF}            Tigrinya Input Method|  
    |*設定*|設定使用者的更新選項。 使用下列其中一個值：<br /><br /> **-所有**等於使用建議的設定。<br /><br /> **-更新**等於安裝重要更新。 只有<br /><br /> **-None**等於不會檢查更新。|  
    |*使用者名稱*|的安裝期間建立的新系統管理員帳戶名稱。 您的系統管理員及標準使用者帳戶名稱必須符合下列準則：<br /><br /> -可達 19 個字元長。<br /><br /> - Cannot contain / \  [ ] &#124; < > + = ; , ? *<br /><br /> -必須啟動，或以句號結尾。<br /><br /> -不能包含兩個連續的句號。<br /><br /> -不能相同的伺服器名稱或內部網域名稱。<br /><br /> -不能做為預先定義的使用者名稱，例如系統管理員或來賓相同。|  
    |*PlainTextPassword*|此為安裝期間建立的新系統管理員帳戶的密碼。<br /><br /> -必須是至少 8 個字元。<br /><br /> -必須包含至少三種下列四類：<br /><br /> 大寫字元。<br /><br /> -小寫字元。<br /><br /> -數字。<br /><br /> -符號。|  
    |*StdUserName*|在安裝期間新建立之標準使用者帳戶的名稱。 如需相關需求，請參閱 *UserName* 參數。|  
    |*StdUserPlainTextPassword*|在安裝期間建立之標準使用者帳戶的密碼。|  
    |WebDomainName|(選用) 設定伺服器的網際網路網域名稱。 此檔案可讓您設定網域名稱，其方法與在網域名稱設定精靈中進行手動設定使用的方法類似。|  
    |TrustedCertFileName|(選用) 為網域名稱設定受信任的憑證。 這可讓您放置 .PFX 憑證，其中包含私密金鑰。|  
    |TrustedCertPassword|(選用) 用來匯入 .PFX 的密碼。|  
    |EnableVPN|(選用) 預設會開啟 VPN。|  
    |VpnIPv4StartAddress|(選用) 設定 VPN 啟動位址。|  
    |VpnIPv4EndAddress|(選用) 設定 VPN 結束位址。|  
    |VpnBaseIPv6Address|(選用) 設定用於 VPN 的基本 IPV6 位址。|  
    |VpnIPv6PrefixLength|(選用) 設定 VPN IPv6 位址的前置長度。|  
    |IsHosted|(選用) 如果未指定，預設值為 false。 如果是在主機服務提供者環境中設定該項目，請設定此值。 它將停用路由器設定。|  
    |StaticIPv4Address|(選用) 如果要設定靜態 IP 位址，而不是動態位址，請指定靜態 IP 位址。|  
    |StaticIPv4Gateway|(選用) 如果要設定靜態 IP 位址，而不是動態位址，請指定預設閘道位址。|  
    |StaticIPv4SubnetMask|(選用) 如果要設定靜態 IP 位址，而不是動態位址，請指定子網路遮罩。|  
    |StaticIPv6Address|(選用) 如果要設定靜態 IP 位址，而不是動態位址，請指定預設 IP 位址。|  
    |StaticIPv6SubnetPrefixLength|(選用) 如果要設定靜態 IP 位址，而不是動態位址，請指定預設的 IPV6 子網路前置碼長度。|  
    |StaticIPv6Gateway|(選用) 如果要設定靜態 IP 位址，而不是動態位址，請指定預設閘道位址。|  
    |ClientBackupOn|(選用) 在新用戶端已聯結伺服器時，根據預設關閉用戶端備份。|  
    |FileHistoryOn|(選用) 在執行 Windows 8 Consumer Preview 的新用戶端已聯結伺服器時，根據預設關閉檔案歷程記錄備份。|  
    |EnableRWA|安裝 Windows Server Essentials 時，它會啟用遠端 Web 存取，但會跳過路由器設定。 這只有在全新安裝產品時才受支援。 預設值為 false。|  
    |IPv4DNSForwarder|設定 IPv4 DNS 轉寄站。|  
    |IPv6DNSForwarder|設定 IPv6 DNS 轉寄站。|  
    |LaunchPadHiddenTasks|-（選擇性） 您可以隱藏備份項目或 / 及啟動列 上的管理儀表板項目。<br /><br /> -若要停用儀表板：LaunchPadHiddenTasks=Microsoft.LaunchPad.AdminDashboard<br /><br /> -若要停用備份：LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup<br /><br /> -若要停用備份和儀表板：LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup,Microsoft.LaunchPad.AdminDashboard|  
  
3.  儲存檔案。 確定您將檔案儲存為 cfg.ini，而不是 cfg.ini.txt。  
  
    > [!NOTE]
    >  您可以將檔案儲存到 USB 快閃磁碟機 (以在特定安裝階段使用)，但 cfg.ini 檔案也可以在目標伺服器上任何硬碟的根目錄中找到。 您必須確定檔案的編碼已設定為 ANSI 或 Unicode，UTF-8 編碼不受支援。  
  
> [!IMPORTANT]
>  只有當使用者想要利用自動安裝回應檔案將伺服器個人化，或合作夥伴想要利用自動安裝回應檔案來測試伺服器帶來的使用者經驗時，才應使用 cfg.ini 中的初始設定區段。 檔案的這個區段目的並非用於建立映像的用途。  
  
## <a name="see-also"></a>另請參閱  

 [與 Windows Server Essentials ADK 快速入門](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)

 [與 Windows Server Essentials ADK 快速入門](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](../install/Additional-Customizations.md)   
 [準備用於部署的映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](../install/Testing-the-Customer-Experience.md)

