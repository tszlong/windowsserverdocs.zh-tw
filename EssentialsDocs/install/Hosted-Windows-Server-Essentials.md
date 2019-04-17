---
title: "主控的 Windows Server Essentials"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fda5628c-ad23-49de-8d94-430a4f253802
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 23e586c22ca14af7b02550e2fa1c9522e379170c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="hosted-windows-server-essentials"></a>主控的 Windows Server Essentials

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

本文件包含主機人想要在他們 lab 部署 Windows Server Essentials 和即服務提供其針對 Windows Server Essentials 服務提供者的特定的資訊。  
  
## <a name="what-is-windows-server-essentials"></a>Windows Server Essentials 為何？  
 Windows Server Essentials 是跨先小型企業方案，並包含最佳的優勢，64 位元 product 技術，提供也適用於小型企業大部分伺服器的環境。 Windows Server Essentials 包含下列技術。  
  
 **伺服器作業系統中：** Windows Server 2012 product 技術提供 Windows Server Essentials 的核心。 如需詳細資訊，請造訪[Windows Server 2012 網站](https://www.microsoft.com/en-us/server-cloud/products/windows-server-2012-r2/default.aspx#fbid=ZH0GD_CRAWh)。  
  
 **資料保護：** Windows Server Essentials 運用提供大幅改進的資料保護功能的 Windows Server 2012 中提供幾個新功能。 [儲存空間的新功能](https://technet.microsoft.com/library/hh831739.aspx)可讓您聚集實體的儲存空間容量的不同的硬碟，動態加入硬碟，並且建立資料磁碟區，彈性的指定層級。 Windows Server Essentials 可以進行完整的系統備份和本身以及連上網路的 client 電腦的伺服器會還原極金屬？ 現在具有超過 2 TB 磁碟區的支援。 新的 Windows Server 2012， [Windows Azure Online Backup](https://technet.microsoft.com/library/hh831419.aspx)可以用來保護的檔案和資料夾中的雲端式存放裝置服務由 Microsoft。 Windows Server Essentials 也集中管理和設定檔歷史功能的 Windows 8.1 用，以協助使用者的檔案不小心刪除或覆寫復原而不需要系統管理員尋求協助。  
  
 **隨處存取：**遠端 Web 存取提供精簡、 方便觸控的瀏覽經驗您從幾乎任何地方存取應用程式與資料有網際網路連接和使用幾乎所有裝置。 Windows Server Essentials 也提供更新的 Windows Phone app 和新的應用程式適用於 Windows 8.1 client 的電腦，讓使用者直接連接到，在搜尋的檔案和資料夾的伺服器上的存取。 檔案也會自動進行離線存取快取，並可連接至伺服器時同步。 Windows Server Essentials 會設定 virtual 私人網路 (VPN) 按幾下，輕鬆、 精靈導向過程中，並且可簡化的使用者存取 VPN 管理。 Client 電腦，可以利用遠端加入 「 Windows SBS 環境，而不需要 office 通勤 VPN 連接。  
  
 **工作負載彈性：** Windows Server Essentials 的設計可以彈性針對選擇哪些應用程式與服務執行場所和，在雲端中執行。 在舊版，Windows 小型企業伺服器標準包含 Exchange Server 隨著元件的費用與複雜新增針對希望運用雲端式傳送簡訊和共同作業服務。 與 Windows Server Essentials，針對可以利用相同類型的整合式的管理體驗是否選擇執行 Exchange Server 先複本，希望裝載換貨的服務，或希望 Microsoft Office 365。  
  
 **健康 Monitoring:** Windows Server Essentials 會監視自己的健康狀態，並 client 電腦執行 Windows 8.1、 Windows 7 和 Mac OS X 版本 10.5 及更新版本的狀態。 健康狀態，通知您的問題或電腦備份、 儲存伺服器、 磁碟空間太少，及更多的相關問題。  
  
 **擴充性：** Windows Server Essentials 的組建上的 Windows SBS 2011 Essentials，這可讓其他軟體供應商核心 product 以新增功能及功能，並新增全新的 [網路服務 Api 擴充性模型。 它也會保留現有的相容性[軟體開發套件](https://msdn.microsoft.com/library/gg513958.aspx)(SDK) 及[增益集](https://pinpoint.microsoft.com/applications/search?fpt=300105&q=small+business+server+essentials)建立 Windows SBS 2011 程式集。  
  
## <a name="how-can-i-customize-an-image"></a>如何自訂的影像？  
 請參考[Windows Server Essentials](https://go.microsoft.com/fwlink/p/?LinkID=249124)，這是與其他 Windows Server Essentials 自訂步驟標準 Windows Server sysprep 程序。 若要完成的自訂項目，請依照[建立簡單自訂映像](https://technet.microsoft.com/en-us/library/jj200117)和[自訂映像](https://technet.microsoft.com/en-us/library/jj200161)，並依照指示進行[準備部署映像](https://technet.microsoft.com/en-us/library/jj200142)擷取您最後的映像。  
  
 您應該會密切注意下列重點：  
  
1.  您應該將 SkipIC.txt 檔案新增至所有磁碟機的根略過初始設定 (IC)。 安裝之前 IC，server 之後按 Shift + F10 上市 cmd 視窗，並建立 c: SkipIC.txt 檔案 / 磁碟機。 自訂項目之後, 您必須記得 delete SkipIC.txt 檔案。  
  
2.  如果您需要 Windows Server Essentials 部署小於 90 GB 磁碟上，您應該會新增登錄按鍵 sysprep 之前：  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
 Sysprep 之後, 您可以使用 sysprepped 磁碟映像或重新封裝回來 Install.wim 到新的部署。  
  
 如果您使用管理員一樣，您可以建立範本使用執行個體。 建立範本執行個體將 sysprep，並關閉伺服器。 您將它儲存在您的媒體櫃之後，您必須將依案例為基礎的執行個體。  
  
##  <a name="BKMK_automatedeployment"></a>如何將自動化部署？  
 取得自訂映像之後，您可以使用您自己的影像執行部署。 為了執行 semiunattended 安裝，您需要提供/部署 unattend.xml\ WinPE 設定。 要完全自動的安裝時，您也需要提供 Windows Server Essentials 的初始設定 cfg.ini 檔案。  
  
1.  執行只自動的 WinPE 安裝。 這將會自動只 WinPE 安裝並安裝之前，先停止初始設定，以便終端使用者可以 Corp、 網域及系統管理員身分的資訊來提供本身之後 RDP 到伺服器工作階段。 若要這樣做：  
  
    1.  提供 Windows unattend.xml\ 檔案。 請遵循[Windows 8.1 ADK](https://go.microsoft.com/fwlink/?LinkId=248694)來產生檔案，並提供所有所需的資訊，包括伺服器名稱、 product 金鑰和系統管理員密碼。 在 unattend.xml\ 檔案的 Microsoft Windows 安裝程式區段中，提供下列資訊。  
  
        ```  
        <InstallFrom>  
                 <MetaData>  
                     <Key>IMAGE/WINDOWS/EDITIONID</Key>  
                     <Value>ServerSolution</Value>  
                 </MetaData>  
                 <MetaData>  
                     <Key>IMAGE/WINDOWS/INSTALLATIONTYPE</Key>  
                     <Value>Server</Value>  
                 </MetaData>  
           </InstallFrom>  
        ```  
  
    2.  RDP 連接埠 3389，必須先在公用 IP，則可以使用系統管理員身分和 RDP unattend.xml\ 檔案伺服器中指定的密碼才能完成初始設定  
  
    > [!NOTE]
    >  如果您不要變更預設的密碼，伺服器安裝將會停止要求輸入密碼的在螢幕上。**注意**終端使用者必須使用預設的系統管理員 account 來登入伺服器，並執行初始設定。  
  
 如果您使用管理員一樣，您可以系統管理員密碼在主機中當您建立新執行個體從範本。  
  
1.  執行包括自動初始設定的自動的安裝完成。 若要這樣做：  
  
    1.  上述一樣部署開始從 WinPE 安裝時，提供 unattend.xml\ 檔案。  
  
    2.  Windows Server Essentials ADK 一節，請參考[建立 Cfg.ini 檔案](https://technet.microsoft.com/en-us/library/jj200150)，以產生 cfg.ini。  
  
    3.  提供 [InitialConfiguration] 中的資訊。  
  
        ```  
        WebDomainName=yourdomainname  
        TrustedCertFileName=c:\cert\a.pfx  
        TrustedCertPassword=Enteryourpassword  
        EnableVPN=true  
        EnableRWA=true  
        ; Provide all information so that after setup is complete, your customer can use your domain name to visit the server directly with the admin/user information you provide in the [InitialConfiguration] section.  
  
        VpnIPv4StartAddress=<IPV4Address>  
        VpnIPv4EndAddress=<IPV4Address>  
        VpnBaseIPv6Address=<IPV6Address>  
        VpnIPv6PrefixLength=<number>  
        ; Provide this information. IPv4StartAddress and IPv4Endaddress are required so that your VPN client can acquire valid IP through this range.  
  
        IPv4DNSForwarder=<IPV4Address,IPV4Address,Â¦>  
        IPv6DNSForwarder=<IPV6Address,IPV6Address,Â¦>  
        ; Provide this information as needed according to your network environment settings.  
        ```  
  
    4.  提供 [PostOSInstall] 中的資訊。  
  
        ```  
        IsHosted=true   
        ; Must have, this will prevent Initial Configure webpage available for other computers under same subnet.  
  
        StaticIPv4Address=<IPV4Address>  
        StaticIPv4Gateway=<IPV4Address>  
        StaticIPv6Address=<IPV6Address>  
        StaticIPv6SubnetPrefixLength=<number>  
        StaticIPv6Gateway=<IPV6Address>  
        ; All these are optional if you have DHCP Server Service on the subnet, otherwise provide static IP here.  
        ```  
  
    5.  如果您提供 WebDomainName 參數，請確定 DNS 記錄也正在指向伺服器 s 公用 IP 更新。  
  
    6.  如果您未提供上述 WebDomainName 資訊，請務必，針對可以連接到伺服器，並完成 VPN 設定使用 RDP 開放 3389 連接埠。  
  
> [!NOTE]
>  請確定 VM 主機 server 與 Windows Server Essentials VM 時區設定為相同。 否則，您可能會遇到數個不同錯誤 (初始設定可能無法在憑證相關工作; 的憑證可能不適用於之後安裝的幾個小時; 裝置資訊不會更新正確; 等等)。  
  
 部署之後，請在來驗證您是否已成功初始設定 HKLM\software\microsoft\windows server\setup 下列機碼。 如果 SetupStage = ICDone 與與 ICStatus = 1，這表示成功完成初始設定。  
  
## <a name="what-is-the-supported-network-topology"></a>支援的網路拓撲為何？  
 若要使用 Windows Server Essentials 的漫遊 client，應該支援 VPN。 我們建議您，讓 VPN SSTP 連接埠 443。 如果需要媒體伺服器功能遠端 Web 存取或 Web 服務的 app，應該也支援連接埠 80。  
  
 VPN 就可以透過我們的 Windows PowerShell 指令碼的自動部署期間完成或您可以使用我們精靈中設定在初始設定後。  
  

-   若要讓 VPN 自動部署期間，請查看[如何自動部署？](Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment)在此文件。  

-   若要讓 VPN 自動部署期間，請查看[如何自動部署？](../install/Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment)在此文件。  

  
-   讓 Windows PowerShell 透過 VPN、 執行下列 cmdlet 以系統管理員權限，並提供所需的所有資訊。  
  
    ```  
    ##  
    ## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).  
    ##  
  
    $myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server  
    $mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file  
    $mySslCertificatePassword = ConvertTo-SecureString '******';   ## password for private key of the SSL certificate  
    $skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate  
  
    Add-Type -AssemblyName 'Wssg.Web.DomainManagerObjectModel';  
    [Microsoft.WindowsServerSolutions.RemoteAccess.Domains.DomainConfigurationHelper]::SetDomainNameAndCertificate($myExternalDomainName,$mySslCertificateFile,$mySslCertificatePassword,$skipCertificateVerification);  
    ##  
    ## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).  
    ##  
  
    Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;  
    ```  
  
 如果您無法提供 VPN 連接送給針對伺服器之前，確定伺服器連接埠 3389 存取網際網路，讓使用者可以使用 RDP 連接到伺服器，並自動執行設定。  
  
 以下是兩個一般伺服器端網路拓撲和 VPN/遠端 Web Access (RWA) 可設定的方式：  
  
-   拓撲 1 （建議選項）  
  
    -   伺服器 virtual NAT 裝置在不同網路中。  
  
    -   DHCP 服務可以在 virtual 網路，或使用靜態 IP 位址指派伺服器。  
  
    -   伺服器連接埠 443 到達。 公用 IP 連接埠 443 從  
  
    -   VPN 傳遞允許 443 連接埠。  
  
    -   應該會在相同的伺服器的位址子網路攻擊距離 VPN IPv4 位址集區。  
  
    -   應該指派給任何第二部伺服器靜態 IP 子網路相同，但退出 VPN 位址集區。  
  
-   拓撲 2:  
  
    -   伺服器有私人 IP 位址。  
  
    -   連接埠 443 伺服器上的到達。 從公用 IP 位址 s 連接埠 443  
  
    -   VPN 傳遞允許 443 連接埠。  
  
    -   VPN IPv4 位址集區位於各種不同的伺服器位址。  
  
 使用拓撲 2，不支援第二個伺服器案例。  
  
## <a name="how-do-i-perform-common-tasks-via-windows-powershell"></a>如何執行一般工作透過 Windows PowerShell？  
 **可讓遠端存取**  
  
```  
Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
```  
  
 範例：  
  
```  
$Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
```  
  
 這個命令將讓遠端 Web Access 與路由器會自動設定，並變更所有現有的使用者的預設的存取權限。  
  
 **將使用者新增**  
  
```  
Add-WssUser [-Name] <string> [-Password] <securestring> [-AccessLevel <string> {User | Administrator}] [-FirstName <string>] [-LastName <string>] [-AllowRemoteAccess] [-AllowVpnAccess]   [<CommonParameters>]  
```  
  
 範例：  
  
```  
$password = ConvertTo-SecureString "Passw0rd!" -asplaintext  œforce  
$Add-WssUser -Name User2Test -Password $password -Accesslevel Administrator -FirstName User2 -LastName Test  
```  
  
 這個命令，將會新增系統管理員密碼 Passw0rd 名 User2Test。  
  
 **停用讓/使用者**  
  
 範例：  
  
```  
$CurrentUser = get-wssuser  œname user2test  
$CurrentUser.UserStatus = 0  
$CurrentUser.Commit()  
```  
  
 名 user2test 使用者停用這個命令。 設定為 1 UserStatus 會讓使用者。  
  
 **新增資料夾伺服器**  
  
```  
Add-WssFolder [-Name] <string> [-Path] <string> [[-Description] <string>] [-KeepPermissions] [<CommonParameters>]  
```  
  
 範例：  
  
```  
$Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
```  
  
 這個命令，將會新增伺服器名 MyTestFolder 在指定的位置。  
  
## <a name="how-do-i-add-a-second-server-to-the-windows-server-essentials-domain"></a>如何在 Windows Server Essentials 的網域新增第二部伺服器？  
 因為 Windows Server Essentials 的網域控制站，您可以加入網域標準的方式來第二個伺服器。  
  
## <a name="which-email-solutions-can-be-integrated"></a>整合的電子郵件方案？  
 Windows Server Essentials 支援使用另外兩個郵件方案整合： Office 365 和先換貨。 如果您執行的自行裝載的電子郵件方案，您將需要開發與您的電子郵件裝載的方案整合 Windows Server Essentials 的增益集。  
  
## <a name="how-do-i-migrate-on-premises-windows-sbs-201120082003-to-the-hosted-windows-server-essentials"></a>如何將先 Windows SBS (2011年日 2008年日 2003) 移轉到 Windows Server Essentials 裝載？  
 移轉指南可先 Windows 小型企業伺服器 (Windows SBS) 以 Windows Server Essentials 移轉。 一些步驟可能不適用於一樣您裝載的環境。 不過，一般工作和移轉的工作負載應該相同。 我們建議您指向[移轉指南](https://go.microsoft.com/fwlink/p/?LinkID=254292)，請根據您的主機環境必要的自訂項目。  
  
 建議您將來源伺服器和目的地伺服器相同子網路中。 如果這不是可能，您應該請確認：  
  
-   來源伺服器和目的地伺服器內部 DNS 名稱都可連接到彼此的。  
  
-   必要的連接埠的開放。  
  
## <a name="how-can-i-upgrade-windows-server-essentials-to-windows-server-standard"></a>如何升級到 Windows Server 標準 Windows Server Essentials？  
 Windows Server Essentials 可以升級至 Windows 伺服器標準。 移除鎖定和限制，並新增的套件遺失的 Windows Server 標準。 如需詳細資訊，[下載文件](https://go.microsoft.com/fwlink/p/?LinkID=253181)。  
  
## <a name="what-are-the-native-tools-for-monitoring-and-management"></a>原生監視和管理工具為何？  
  
### <a name="group-policy-management"></a>群組原則管理  
 Windows Server Essentials 運用原生群組原則支援 Windows Server 2012 中的，並提供使用者介面設定資料夾重新導向和安全性設定。  
  
> [!NOTE]
>  在裝載環境中，是否可以使用資料夾重新導向的使用者設定檔，它可能會增加終端使用者來登入資料大小時大的時間。  
  
### <a name="management-pack"></a>管理組件  
 Windows Server Essentials 管理組件會提供監視功能健康警示系統的 Windows Server Essentials 可協助主機服務提供者管理大量致力於公司不同小型企業的 Windows Server Essentials 伺服器上。 在此版本的監視包含系統只重要警示。  
  
#### <a name="management-pack-scope"></a>管理組件範圍  
 此管理組件可以協助您監視特定 Windows Server Essentials 的功能。 這不會監視的一般的 Windows Server 2012 標準的作業系統功能。 為了監視 Windows Server Essentials，您應該使用 Windows Server Essentials 管理組件和管理組件的 Windows Server 2012 標準。  
  
#### <a name="mandatory-configuration"></a>必要的設定  
 需要會引導您可以使用管理組件之前下列步驟：  
  
1.  安裝代理程式並設定信任使用信任的憑證。 Windows Server Essentials 已預先設定為網域控制站並不能與其他網域或森林信任，因為系統中心作業管理員代理程式應該要安裝 Windows Server Essentials 上與設定的管理伺服器使用憑證的信任。  
  
2.  下載管理組件。 若要使用 Operations Manager 2007 監視 Windows Server Essentials，您必須先下載[Windows Server 作業系統的管理組件](https://connect.microsoft.com/WindowsServer/Downloads/DownloadDetails.aspx?DownloadID=45010)來自管理組件 Catalog。  
  
3.  匯入管理組件的檔案。 如果您使用當地語系化的管理組件，您要匯入管理主要套件檔案和語言套件。  
  
#### <a name="files-in-this-monitoring-pack"></a>在此監視封包檔案  
 監視套件適用於 Windows Server Essentials 包括下列檔案：  
  
-   Microsoft.Windows.Server.2012.Essentials.mp  
  
-   .Mp Microsoft.Windows.Server.2012.Essentials。 < locale\ >  
  
### <a name="back-up-and-restore"></a>備份及還原  
 Windows Server Essentials 可讓您同時伺服器及 client 備份。  
  
#### <a name="back-up-the-server"></a>備份伺服器  
 Windows Server Essentials 支援兩種備份伺服器： 先備份與關閉場所備份。  
  
 **先備份**可讓您以不同的磁碟定期執行封鎖層級增量備份。 項，以，可以將 virtual 磁碟附加到 Windows Server Essentials VM，設定此 virtual 磁碟伺服器備份。 Virtual 磁碟必須位於與 Windows Server Essentials VM 的其他實體磁碟。  
  
-   如果您有另一個備份 Windows Server Essentials VM 機制，並不想讓您的使用者，查看 Windows Server Essentials 的原生伺服器的備份功能，您可能會將它關閉和移除所有的相關的使用者介面從 Windows Server Essentials 儀表板。 如需詳細資訊，請參考自訂伺服器備份區段[ADK 文件](https://go.microsoft.com/fwlink/p/?LinkID=249124)。  
  
 **關閉場所備份**可讓您可以定期備份至雲端服務 server 的資料。 您可以下載並安裝 Microsoft Azure 備份整合模組適用於 Windows Server Essentials 利用 Microsoft 所提供的 Azure 備份。  
  
 如果您或您的使用者想要另一個雲端服務，您應該：  
  
1.  更新 Windows Server Essentials 儀表板的使用者介面讓它提供的連結，以您慣用的雲端服務，而不是預設 Azure 備份。 如需詳細資訊，請參考自訂映像一節[ADK 文件](https://go.microsoft.com/fwlink/p/?LinkID=249124)。  
  
2.  （選擇性）開發適用於 Windows Server Essentials 儀表板設定及管理雲端服務備份增益集。  
  
#### <a name="back-up-the-client"></a>備份 client  
 Windows Server Essentials 支援兩種 client 資料備份： client 完整備份與歷史檔案。  
  
> [!NOTE]
>  備份 client 可能會影響的效能，因為資料必須 client 的伺服器來傳送透過 VPN。  
  
 **完整 client 備份**預設會在適用於所有 client 裝置連接到 Windows Server Essentials 的網路。 它會累加備份完整 client （系統和資料），並支援 deduplication 資料。 備份資料將會執行 Windows Server Essentials 的伺服器上。 失敗的 client 可以取得回復至先前的備份點及其資料。 您可以關閉此功能依照下列步驟建立 Cfg.ini 檔案區段中的[ADK 文件](https://technet.microsoft.com/en-us/library/jj200150)。  
  
 Client 完整備份的一些注意事項︰  
  
-   效能： 初始 client 備份可能會花時間而的資料會上傳至量。  
  
-   穩定性： 有時候連接網際網路不穩定 client 一邊。 Client 備份設計為可繼續，而且預設檢查點 40 GB （每一次備份 40 GB 資料 client 備份資料庫會建立檢查點）。 如果您希望是不可靠連接網際網路，您可以變更此值為較小的數字。  
  
    -   若要檢查點工作，在伺服器上，可讓設定登錄 HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs 1。  
  
    -   若要檢查點上 client，閾值來改善會變更 HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold 預設值 (40 GB 以上)。  
  
-   枯金屬還原 client: Windows 預先安裝環境不支援 VPN 連接，因為 Client 枯的金屬還原不支援。  
  
 **檔案歷史**在網路共用是 Windows 8.1 功能的資料備份設定檔 （媒體櫃、 桌面、 連絡人、 [我的最愛]）。 在 Windows Server Essentials，我們將允許檔案歷史設定的所有 Windows 8.1 用加入 Windows Server Essentials 的集中管理。 執行 Windows Server Essentials 的伺服器上儲存的備份資料。 您可以關閉此功能依照下列步驟建立區段 Cfg.ini 檔案[ADK 文件](https://technet.microsoft.com/en-us/library/jj200150)。  
  
### <a name="storage-management"></a>管理儲存空間  
 [儲存空間的新功能](https://technet.microsoft.com/library/hh831739.aspx)可讓您聚集實體的儲存空間容量的不同的硬碟，動態加入硬碟，並且建立資料磁碟區，彈性的指定層級。 您也可以附加到以展開其儲存體 Windows Server Essentials 的 iSCSI 磁碟。  
  
## <a name="what-are-the-main-scenarios-i-should-test"></a>我應該測試的案例主要有哪些？  
 控管觀點，建議您測試案例下列動作：  
  
 **伺服器部署**  
  
-   部署 Windows Server Essentials 伺服器 lab 環境中。  
  
-   視需要來自訂 Windows Server Essentials 映像。  
  
-   將 [自動的檔案 cfg.ini 與 Windows Server Essentials 部署。  
  
-   將在場所 Windows SBS 移轉到主控 Windows Server Essentials。  
  
-   Windows Server Essentials 的升級到 Windows Server 2012。  
  
 **伺服器設定**  
  
-   設定任何地方存取 (VPN、 遠端網路存取權，DirectAccess)。  
  
-   將設定儲存空間 」 和 「 伺服器資料夾。  
  
-   （如果適用）設定伺服器的備份，Online Backup Client 備份、 歷史檔案。  
  
-   （如果適用）設定及管理儲存空間。  
  
-   （如果適用）設定電子郵件方案整合 （Office 365、 裝載換貨，以及等）。  
  
-   （如果適用）設定媒體伺服器。  
  
 **伺服器管理**  
  
-   管理使用者。  
  
-   設定並通知的警示的電子郵件。  
  
-   執行 BPA 在錯誤日警告。  
  
-   設定 System Center 監視套件。  
  
-   設定伺服器復原，在損壞。  
  
 **Client 的體驗**  
  
-   透過網際網路 （個人電腦或 Mac OS） client 部署。  
  
-   使用上 client Launchpad 存取共用資料夾。  
  
-   存取伺服器資產遠端網路存取，透過從其他裝置 （個人電腦、 手機、 平板電腦）。  
  
-   適用於 Windows Phone 我伺服器的 app。  
  
-   （如果適用）歷史、 Client 備份與還原 (不 BMR，) 資料夾重新導向檔案。  
  
-   （如果適用）電子郵件整合的體驗。  
  
## <a name="where-can-i-get-more-support"></a>何處取得更多支援？  
 您可以從下列連結，以取得 SDK 和 ADK 文件：  
  
-   [SDK](https://go.microsoft.com/fwlink/p/?LinkID=248648)  
  
-   [ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124)  
  
 您可以透過連接功能小組以報告錯誤。 若要產生登，壓縮加入伺服器戶端和伺服器上的資料夾： C:\ProgramData\Microsoft\Windows Server\Logs。
