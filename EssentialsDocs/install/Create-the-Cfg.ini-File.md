---
title: 建立 Cfg.ini 檔案
description: 瞭解如何建立用來自動安裝作業系統的 cfg.ini 檔案。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 93a73556-22ef-402d-b8d4-582b74c22bcf
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: ffc9ecaf6059ab21aae474d211ab397858f1b7d2
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711223"
---
# <a name="create-the-cfgini-file"></a>建立 Cfg.ini 檔案

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

您可以在下列情況中使用 cfg.ini 檔案來自動安裝作業系統：

-   在目標電腦上使用預先安裝的映像來測試使用者的經驗時，可使用初始設定區段以自動或非自動安裝模式，逐步完成安裝作業。 若要執行此動作，請參閱[建立初始設定區段](Create-the-Cfg.ini-File.md#BKMK_CreateInit2)。

##  <a name="create-the-initial-configuration-section"></a><a name="BKMK_CreateInit2"></a> 建立初始設定區段
 使用 cfg.ini 檔案中的初始設定區段，以手動或自動模式逐步完成安裝。

#### <a name="to-define-the-initial-configuration-section"></a>若要定義初始設定區段

1.  在 [記事本] 中開啟 cfg.ini 檔案（如果有的話）;否則，請建立新的檔案。

2.  新增下列文字以建立 InitialConfiguration 區段。

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
    >  未提供在初始設定期間選取不同語言的選項。 如果重設系統，則作業系統的語言會是原始安裝的語言。

    |參數名稱|參數描述|
    |--------------------|---------------------------|
    |*AcceptEula*|指出使用者接受 Microsoft 軟體授權合約。 值可以等於 True 或 False，但是只有在此值設定為 True 時，才會繼續安裝。|
    |*AcceptOEMEula*| (選擇性) 表示使用者接受合作夥伴授權合約。 值可以等於 True 或 False。 只有向提供個別授權合約的合作夥伴購買伺服器時，才需填入此欄位。|
    |*CompanyName*|(選用) 公司的名稱。 您的公司名稱是用來將您的伺服器與公司建立關聯，並自訂您的公司報告。 長度最多可達254個字元。|
    |*國家/地區*|(選用) 字串表示要選擇的國家/地區。 例如：US 代表美國。|
    |*ServerName*|伺服器名稱可唯一識別網路上的伺服器。 您的伺服器名稱必須符合下列準則：<br /><br /> -長度最多可以有15個字元。<br /><br /> -可以包含字母、數位和連字號 (-) 。<br /><br /> -不得以連字號開頭。<br /><br /> -不得包含任何空格。<br /><br /> -不得只包含數位。<br /><br /> 例如：ContosoServer。|
    |*DNSName*|內部網域會將伺服器和用戶端電腦群組在一起，以共用使用者名稱、密碼和其他通用資訊的通用資料庫。 使用者在登入電腦時會看見此名稱，但此名稱僅供內部使用，而且與網際網路的網域名稱不同。 內部網域名稱必須符合針對 *ServerName* 指定的相同準則。<br /><br /> 例如：contoso.local。|
    |*NetbiosName*|NetBIOS 名稱可用來識別伺服器上所執行的資源。 其長度最多可達 15 個字元。 範例： Contoso。|
    |*Language*| (選擇性) 指定顯示語言。 它只能是其中一個已安裝的語言。 範例：美國地區使用的英文版。|
    |*地區設定*|(選擇性) 使用 *LocaleID* 格式指定時間與貨幣格式。 範例： en-us （以英文顯示的貨幣和時間為單位），並根據美國所使用的標準進行格式化。|
    |*鍵盤*|鍵盤可以採用下列兩種格式：<br /><br /> - **輸入語言：鍵盤配置。** 例如 0409:00000409，其中 **:** 之前的 0409 是輸入語言，而 **00000409** 是鍵盤配置。 您可以在 **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Keyboard Layouts** 登錄機碼下找到鍵盤配置清單。<br /><br /> - **輸入語言： IME 識別碼。** 以下是 IME 識別碼的完整清單。<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {8F96574E-C86C-4bd6-9666-3F7327D4CBE8} 的阿姆哈拉輸入方法<br /><br /> -{81d4e9c9-1d3b-41bc-9e6c-4b40bf79e35e} {FA550B04-5AD7-411F-A5AC-CA038EC515D7} Microsoft 拼音-簡單快速 (簡體中文) <br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732} {B2F9C502-1742-11D4-9790-0080C882687E} 中文 (傳統) -新注音<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732} {4BDF9F03-C7D3-11D4-B2AB-0080C882687E} 中文 (傳統) -倉頡<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732} {6024B45F-5C54-11D4-B921-0080C882687E} 中文 (傳統) -快速<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {D38EFF65-AA46-4FD5-91A7-67845FB02F5B} 中文繁體陣列<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {037B2C25-480C-4D7F-B027-D6CA6B69788A} 中文傳統 DaYi<br /><br /> -{03B5835F-F03C-411B-9CE2-AA23E1171E36} {A76C93D9-5523-4E90-AAFA-4DB112F9AC76} Microsoft IME (日文) <br /><br /> -{A028AE76-01B1-46C2-99C4-ACD9858AE02F} {B5FE1F02-D5F2-4445-9C03-C568F23C99A1} Microsoft IME (韓文) <br /><br /> -{A1E2B86B-924A-4D43-80F6-8A820DF7190F} {B60AF051-257A-46BC-B9D3-84DAD819BAFB} 舊的韓文 IME (韓文) <br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {409C8376-007B-4357-AE8E-26316EE3FB0D} Yi 輸入方法<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {3CAB88B7-CC3E-46A6-9765-B772AD7761FF} 提提的輸入方法|
    |*設定*|設定使用者選取專案以進行更新。 請使用下列其中一個值：<br /><br /> **-All** 等於使用建議的設定。<br /><br /> **-更新** 等於安裝重要更新。 向<br /><br /> **-無** equals：不檢查更新。|
    |*使用者名稱*|-安裝期間所建立之新系統管理員帳戶的名稱。 系統管理員和標準使用者帳戶名稱必須符合下列準則：<br /><br /> -長度最多可達19個字元。<br /><br /> -不可包含/\ []  &#124; < > + =;, ? *<br /><br /> -不得以句號開頭或結尾。<br /><br /> -不得包含兩個連續的句點。<br /><br /> -不得與伺服器名稱或內部功能變數名稱相同。<br /><br /> -不得與預先定義的使用者名稱相同，例如 Administrator 或 Guest。|
    |*PlainTextPassword*|這是在安裝期間所建立之新系統管理員帳戶的密碼。<br /><br /> ：長度必須至少為8個字元。<br /><br /> -至少必須包含下列四個類別中的三個：<br /><br /> -大寫字元。<br /><br /> -小寫字元。<br /><br /> 型號.<br /><br /> 字元.|
    |*StdUserName*|在安裝期間建立的新標準使用者帳戶的名稱。 如需相關需求，請參閱 *UserName* 參數。|
    |*StdUserPlainTextPassword*|在安裝期間建立之標準使用者帳戶的密碼。|
    |WebDomainName|(選用) 設定伺服器的網際網路網域名稱。 此檔案可讓您設定類似于在功能變數名稱設定向導中用於手動設定之方法的功能變數名稱。|
    |TrustedCertFileName| (選擇性) 為功能變數名稱設定受信任的憑證。 這可讓您放置 .PFX 憑證，其中包含私密金鑰。|
    |TrustedCertPassword|(選用) 用來匯入 .PFX 的密碼。|
    |EnableVPN| (選擇性) 開啟 VPN （預設為開啟）。|
    |VpnIPv4StartAddress| (選擇性) 設定 VPN 開始位址。|
    |VpnIPv4EndAddress| (選擇性) 設定 VPN 結束位址。|
    |VpnBaseIPv6Address|(選用) 設定用於 VPN 的基本 IPV6 位址。|
    |VpnIPv6PrefixLength| (選擇性) 設定 VPN IPv6 位址的前置長度。|
    |IsHosted| (選擇性) 如果未指定，預設值為 false。 如果是在主機服務提供者環境中設定該項目，請設定此值。 它會停用路由器設定。|
    |StaticIPv4Address|(選用) 如果要設定靜態 IP 位址，而不是動態位址，請指定靜態 IP 位址。|
    |StaticIPv4Gateway|(選用) 如果要設定靜態 IP 位址，而不是動態位址，請指定預設閘道位址。|
    |StaticIPv4SubnetMask| (選擇性) 如果您要設定靜態 IP 位址而非動態位址，請指定子網路遮罩。|
    |StaticIPv6Address|如果您想要設定靜態 IP 位址而不是動態位址， (選擇性) 指定預設 IP 位址。|
    |StaticIPv6SubnetPrefixLength|(選用) 如果要設定靜態 IP 位址，而不是動態位址，請指定預設的 IPV6 子網路前置碼長度。|
    |StaticIPv6Gateway|(選用) 如果要設定靜態 IP 位址，而不是動態位址，請指定預設閘道位址。|
    |ClientBackupOn| (選擇性) 在新用戶端加入伺服器時預設關閉用戶端備份。|
    |FileHistoryOn|(選用) 在執行 Windows 8 Consumer Preview 的新用戶端已聯結伺服器時，根據預設關閉檔案歷程記錄備份。|
    |EnableRWA|它會在安裝 Windows Server Essentials 時啟用遠端 Web 存取，但會跳過路由器設定。 只有產品的全新安裝才支援此功能。 預設值為 false。|
    |IPv4DNSForwarder|設定 IPv4 DNS 轉寄站。|
    |IPv6DNSForwarder|設定 IPv6 DNS 轉寄站。|
    |LaunchPadHiddenTasks|- (選擇性) 您可以在啟動控制板上隱藏備份專案或/和系統管理員儀表板專案。<br /><br /> -停用儀表板： LaunchPadHiddenTasks = Microsoft.launchpad.admindashboard<br /><br /> -停用備份： LaunchPadHiddenTasks = Microsoft. 備份<br /><br /> -若要停用備份和儀表板： LaunchPadHiddenTasks = Microsoft.launchpad.admindashboard|

3.  儲存檔案。 請確定您將檔案儲存為 cfg.ini，而不是 cfg.ini.txt。

    > [!NOTE]
    >  您可以將檔案儲存至 USB 快閃磁片磁碟機，該磁片磁碟機可用於安裝的特定階段，或 cfg.ini 檔案可以位於目標伺服器上任何硬碟的根目錄。 您必須確定檔案的編碼設定為 ANSI 或 Unicode，不支援 UTF-8 編碼。

> [!IMPORTANT]
>  只有當使用者想要利用自動安裝回應檔案將伺服器個人化，或合作夥伴想要利用自動安裝回應檔案來測試伺服器帶來的使用者經驗時，才應使用 cfg.ini 中的初始設定區段。 檔案的這個區段不適合用來建立映射。

## <a name="see-also"></a>另請參閱

 [使用 Windows Server ESSENTIALS ADK 消費者入門](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)[建立和自訂映射](Creating-and-Customizing-the-Image.md)[其他自訂](Additional-Customizations.md)專案[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)

