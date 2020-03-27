---
title: 主控的 Windows Server Essentials
description: 說明如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fda5628c-ad23-49de-8d94-430a4f253802
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 76319f87a246c6fabbe0befaf7dc4c74d1416ac4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311752"
---
# <a name="hosted-windows-server-essentials"></a>主控的 Windows Server Essentials

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

本檔包含主控者的特定資訊，這些人員想要在實驗室中部署 Windows Server Essentials，並為其客戶提供 Windows Server Essentials 即服務。  
  
## <a name="what-is-windows-server-essentials"></a>什麼是 Windows Server Essentials？  
 Windows Server Essentials 是一種跨單位的小型企業解決方案，結合了最棒的64位產品技術，為廣大的小型企業提供非常適合的伺服器環境。 Windows Server Essentials 包含下列技術。  
  
 **伺服器作業系統：** Windows Server 2012 產品技術提供 Windows Server Essentials 的核心。 如需詳細資訊，請瀏覽 [Windows Server 2012 網站](https://www.microsoft.com/server-cloud/products/windows-server-2012-r2/default.aspx#fbid=ZH0GD_CRAWh)。  
  
 **資料保護：** Windows Server Essentials 利用 Windows Server 2012 中提供的幾項新功能，大幅提升資料保護功能。 [新的「儲存空間」功能](https://technet.microsoft.com/library/hh831739.aspx) 可讓您彙總不同硬碟的實體儲存容量、以動態方式新增硬碟，並以指定的彈性層級建立資料磁碟區。 Windows Server Essentials 可以執行完整的系統備份以及伺服器本身的裸機還原，以及連線到網路的用戶端電腦，現在支援的磁片區大於 2 TB。 透過 Windows Server 2012 的更新， [Windows Azure Online Backup](https://technet.microsoft.com/library/hh831419.aspx) 可用來在由 Microsoft 所管理的雲端存放服務中保護檔案和資料夾。 Windows Server Essentials 也會集中管理和設定 Windows 8.1 用戶端的檔案歷程記錄功能，以協助使用者在不需要系統管理員協助的情況下，從意外刪除或覆寫的檔案中復原。  
  
 **隨處存取：** 遠端 Web 存取提供簡化、易於使用的瀏覽器體驗，讓您可以從任何有網際網路連線的位置，使用任何裝置來存取應用程式和資料。 Windows Server Essentials 也為 Windows 8.1 用戶端電腦提供更新的 Windows Phone 應用程式和新的應用程式，讓使用者可以直覺地連接、搜尋及存取伺服器上的檔案和資料夾。 系統也會自動快取檔案以供離線存取，並在恢復伺服器連線時進行同步。 Windows Server Essentials 可讓您只需按幾下滑鼠，就能輕鬆地將虛擬私人網路（VPN）設定為簡單的 wizard 驅動程式，並簡化使用者 VPN 存取的管理作業。 用戶端電腦可利用 VPN 連線，不需回到辦公室就可以遠端方式進入 Windows SBS 環境。  
  
 **工作負載彈性：** Windows Server Essentials 的設計可讓客戶彈性地選擇要在內部部署執行的應用程式和服務，以及在雲端中執行的服務。 在之前的版本中，Windows Small Business Server Standard 將 Exchange Server 納入成為元件產品，使得想要使用雲端式傳訊和協同作業服務的客戶必須承擔更多的成本以及複雜性。 透過 Windows Server Essentials，客戶可以利用相同類型的整合式管理體驗，不論他們選擇執行的是 Exchange Server 的內部部署複本、訂閱託管 Exchange 服務，或是訂閱 Microsoft Office 365。  
  
 **健全狀況監視：** Windows Server Essentials 會監視其自身的健康狀態，以及執行 Windows 8.1、Windows 7 和 Mac OS X 10.5 版和更新版本的用戶端電腦狀態。 健康狀態會通知您與電腦備份、伺服器存放裝置和磁碟空間不足等有關的問題。  
  
 擴充性 **：** Windows Server Essentials 建置於 Windows SBS 2011 Essentials 的擴充性模型上，可讓其他軟體廠商將功能和功能新增至核心產品，並新增一組新的 web 服務 Api。 此外，它也可維護與現有 [軟體開發套件](https://msdn.microsoft.com/library/gg513958.aspx) (SDK) 以及為 Windows SBS 2011 Essentials 建立之 [增益集](https://pinpoint.microsoft.com/applications/search?fpt=300105&q=small+business+server+essentials) 之間的相容性。  
  
## <a name="how-can-i-customize-an-image"></a>如何自訂映像？  
 請參閱[Windows Server Essentials](https://go.microsoft.com/fwlink/p/?LinkID=249124)，這是標準的 windows server sysprep 程式，其中包含額外的 Windows server essentials 自訂步驟。 若要完成自訂，請遵循 [建立簡單的自訂映像](https://technet.microsoft.com/library/jj200117) 和 [自訂映像](https://technet.microsoft.com/library/jj200161)中的指示，然後遵循 [準備用於部署的映像](https://technet.microsoft.com/library/jj200142) 中的指示來擷取您的最終映像。  
  
 您應注意以下各項：  
  
1. 您應將 SkipIC.txt 檔案新增至任何磁碟機的根目錄，以略過初始設定 (IC)。 在安裝伺服器之後以及初始設定之前，按 Shift+F10 啟動 cmd 視窗並在 C:/ 磁碟機下方建立一個 SkipIC.txt 檔案。 在完成自訂後，請記得刪除 SkipIC.txt 檔案。  
  
2. 如果您需要在小於 90 GB 的磁片上部署 Windows Server Essentials，您應該在 sysprep 之前新增登錄機碼：  
  
   ```  
   %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
   ```  
  
   在 Sysprep 之後，您可以使用已執行過 Sysprep 的磁碟映像，或將其重新封裝回 Install.wim 進行新部署。  
  
   如果您使用 Virtual Machine Manager，可使用執行中的執行個體來建立範本。 建立範本將會在執行個體上執行 Sysprep 並關閉伺服器。 在儲存至程式庫之後，您就可以依個別情況將執行個體叫出。  
  
##  <a name="how-do-i-automate-the-deployment"></a><a name="BKMK_automatedeployment"></a>如何? 自動化部署？  
 在您取得自訂映像之後，就可以使用您自己的映像來執行部署。 若要執行半自動安裝，您必須提供部署 unattend.xml 以進行 WinPE 設定。 若要執行完全自動安裝，您也需要提供適用于 Windows Server Essentials 初始設定的 cfg .ini 檔案。  
  
1. 僅執行自動 WinPE 設定。 這只會自動化 WinPE 設定，並讓安裝在初始設定之前停止，讓使用者可在 RDP 進入伺服器工作階段之後自行提供 Corp、網域和系統管理員資訊。 若要這樣做：  
  
   1.  提供 Windows unattend.xml 檔案。 遵循[WINDOWS 8.1 ADK](https://go.microsoft.com/fwlink/?LinkId=248694)以產生檔案，並提供所有必要的資訊，包括伺服器名稱、產品金鑰和系統管理員密碼。 在 unattend.xml 檔案的 [Microsoft-Windows 設定] 區段中，提供如下所示的資訊。  
  
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
  
   2.  RDP 埠3389必須在公用 IP 上開啟，讓客戶可以使用 unattend.xml 檔案中指定的系統管理員和密碼，以 RDP 連線到伺服器，以完成初始設定。  
  
   > [!NOTE]
   >  如果您不變更預設密碼，伺服器安裝將會停止，畫面也會要求您輸入密碼。**注意** 使用者必須使用預設系統管理員帳戶以登入伺服器並執行初始設定。  
  
   如果您使用 Virtual Machine Manager，當您從範本建立新的執行個體時，可以在主控台指定系統管理員密碼。  
  
2. 執行包含自動初始設定在內的完整自動設定。 若要這樣做：  
  
   1.  如果部署從 WinPE 設定開始，您可以如同前述提供 unattend.xml 檔案。  
  
   2.  請參閱有權[建立 cfg](https://technet.microsoft.com/library/jj200150)檔案的 Windows SERVER Essentials ADK 一節，以產生 cfg。  
  
   3.  在 [InitialConfiguration] 中提供資訊。  
  
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
  
   4.  在 [PostOSInstall] 中提供資訊。  
  
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
  
   5.  如果您提供 WebDomainName 參數，請確定 DNS 記錄也會更新，以指向伺服器的公用 IP。  
  
   6.  如果您未提供以上的 WebDomainName 資訊，請確定已開啟連接埠 3389，使客戶能使用 RDP 以連線伺服器並完成 VPN 設定。  
  
> [!NOTE]
>  請確定 VM 主機伺服器和 Windows Server Essentials VM 的時區設定相同。 否則您可能會遇到幾種不同的錯誤 (初始設定可能會在憑證相關工作中失敗；憑證在安裝後的數小時之內可能無法運作；裝置資訊無法正確更新等等)  
  
 在完成部署之後，請檢查位於 HKLM\software\microsoft\windows server\setup 下方的下列登錄機碼，以確認初始設定是否成功。 如果 SetupStage == ICDone && ICStatus == 1，表示初始設定已成功完成。  
  
## <a name="what-is-the-supported-network-topology"></a>什麼是支援的網路拓撲？  
 若要從漫遊用戶端使用 Windows Server Essentials，應啟用 VPN。 我們建議您啟用連接埠 443 以進行 VPN SSTP 連線。 如果遠端 Web 存取或 Web 服務等應用程式需要媒體伺服器功能，則應啟用連接埠 80。  
  
 在自動部署期間，VPN 啟用可透過 Windows PowerShell 指令碼完成，或者可在初始設定之後透過精靈設定。  
  

- 若要在自動部署期間啟用 VPN，請參閱本文件中的 [How do I automate the deployment?](Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) 。  

- 若要在自動部署期間啟用 VPN，請參閱本文件中的 [How do I automate the deployment?](../install/Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) 。  

  
- 若要透過 Windows PowerShell 啟用 VPN，請透過系統管理員權限執行以下 Cmdlet，並提供所有必要資訊。  
  
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
  
  如果您無法在客戶存取伺服器之前提供 VPN 連線，請確定您可透過網際網路連線伺服器連接埠 3389，讓使用者可使用 RDP 來連線伺服器並自行執行設定。  
  
  以下是兩種一般的伺服器端網路拓撲，以及 VPN/遠端 Web 存取 (RWA) 的設定方法：  
  
- 拓撲 1 (慣用)  
  
  -   位於 NAT 裝置下方，個別虛擬網路中的伺服器。  
  
  -   已在虛擬網路中啟用 DHCP，或者已使用靜態 IP 位址指派伺服器。  
  
  -   可從公用 IP 連接埠 443 連線伺服器連接埠 443。  
  
  -   連接埠 443 允許 VPN 通道。  
  
  -   VPN IPv4 位址集區應位於伺服器位址的相同子網路。  
  
  -   若有第二個伺服器，則必須指派相同子網路中的靜態 IP，但其應位於 VPN 位址集區之外。  
  
- 拓撲 2：  
  
  -   伺服器具有私人 IP 位址。  
  
  -   可從公用 IP 位址 s 埠443連線到伺服器上的埠443。  
  
  -   連接埠 443 允許 VPN 通道。  
  
  -   VPN IPv4 位址集區位於不同的伺服器位址範圍。  
  
  使用拓撲 2 時，則不支援第二個伺服器案例。  
  
## <a name="how-do-i-perform-common-tasks-via-windows-powershell"></a>如何透過 Windows PowerShell 執行一般工作？  
 **啟用遠端 Web 存取**  
  
```  
Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
```  
  
 範例：  
  
```  
$Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
```  
  
 該命令會使用自動設定的路由器啟用遠端 Web 存取，並變更所有現有使用者的預設存取權限。  
  
 **新增使用者**  
  
```  
Add-WssUser [-Name] <string> [-Password] <securestring> [-AccessLevel <string> {User | Administrator}] [-FirstName <string>] [-LastName <string>] [-AllowRemoteAccess] [-AllowVpnAccess]   [<CommonParameters>]  
```  
  
 範例：  
  
```  
$password = ConvertTo-SecureString "Passw0rd!" -asplaintext  œforce  
$Add-WssUser -Name User2Test -Password $password -Accesslevel Administrator -FirstName User2 -LastName Test  
```  
  
 此命令會新增名為 User2Test 且密碼為 Passw0rd！的系統管理員。  
  
 **啟用/停用使用者**  
  
 範例：  
  
```  
$CurrentUser = get-wssuser  œname user2test  
$CurrentUser.UserStatus = 0  
$CurrentUser.Commit()  
```  
  
 此命令將會停用名為 user2test 的使用者。 將 UserStatus 設為 1 可啟用使用者。  
  
 **新增伺服器資料夾**  
  
```  
Add-WssFolder [-Name] <string> [-Path] <string> [[-Description] <string>] [-KeepPermissions] [<CommonParameters>]  
```  
  
 範例：  
  
```  
$Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
```  
  
 此命令會在指定的位置新增名為 MyTestFolder 的伺服器資料夾。  
  
## <a name="how-do-i-add-a-second-server-to-the-windows-server-essentials-domain"></a>如何? 將第二部伺服器新增至 Windows Server Essentials 網域嗎？  
 由於 Windows Server Essentials 是網域控制站，因此您可以使用標準方式將第二部伺服器加入網域。  
  
## <a name="which-email-solutions-can-be-integrated"></a>可整合哪些電子郵件解決方案？  
 Windows Server Essentials 支援與兩個現成的電子郵件解決方案整合： Office 365 和內部部署 Exchange。 如果您執行自己的主控電子郵件解決方案，您將需要開發增益集來整合 Windows Server Essentials 與您的託管電子郵件解決方案。  
  
## <a name="how-do-i-migrate-on-premises-windows-sbs-201120082003-to-the-hosted-windows-server-essentials"></a>如何? 將內部部署 Windows SBS （2011/2008/2003）遷移至託管的 Windows Server Essentials？  
 遷移指南適用于內部部署 Windows Small Business Server （Windows SBS）到 Windows Server Essentials 的遷移。 某些步驟可能無法完全套用在您的主控環境中。 不過待移轉的一般工作和工作負載應相同。 建議您參閱 [移轉指南](https://go.microsoft.com/fwlink/p/?LinkID=254292) ，並根據您的主控環境來進行必要的自訂。  
  
 建議您將來源伺服器和目的地伺服器置於相同的子網路。 如果此方法不可行，則應確定下列各項：  
  
-   來源伺服器和目的地伺服器可相互取得對方的內部 DNS 名稱。  
  
-   所有必要的連接埠皆已開啟。  
  
## <a name="how-can-i-upgrade-windows-server-essentials-to-windows-server-standard"></a>如何將 Windows Server Essentials 升級至 Windows Server Standard？  
 您可以將 Windows Server Essentials 升級至 Windows Server Standard。 移除鎖定和限制，並新增 Windows Server Standard 缺少的套件。 如需詳細資訊，請 [下載文件](https://go.microsoft.com/fwlink/p/?LinkID=253181)。  
  
## <a name="what-are-the-native-tools-for-monitoring-and-management"></a>可用來進行監視與管理的原生工具有哪些？  
  
### <a name="group-policy-management"></a>群組原則管理  
 Windows Server Essentials 利用 Windows Server 2012 中的原生群組原則支援，並提供使用者介面來設定資料夾重新導向和安全性設定。  
  
> [!NOTE]
>  在主控環境中，如果使用者設定檔已啟用資料夾重新導向，當資料量過大時，使用者可能需要更多時間進行登入。  
  
### <a name="management-pack"></a>管理組件  
 Windows Server Essentials 管理元件在 Windows Server Essentials 中提供健全狀況警示系統的監視功能，可協助主控者管理專屬於不同小型企業公司的大量 Windows Server Essentials 伺服器。 此版本中的監視功能僅包含系統中的重要警示。  
  
#### <a name="management-pack-scope"></a>管理組件範圍  
 此管理元件可協助您監視 Windows Server Essentials 特有的功能。 它不會監視 Windows Server 2012 Standard 作業系統中的一般功能。 為了監視 Windows Server Essentials，您應該使用 windows Server Essentials 管理元件和適用于 Windows Server 2012 Standard 的管理元件。  
  
#### <a name="mandatory-configuration"></a>強制設定  
 在使用管理組件之前，您必須執行以下步驟：  
  
1.  安裝代理程式並使用憑證信任來設定信任層級。 因為 Windows Server Essentials 已預先設定為網域控制站，而且不能與其他網域或樹系信任，所以 System Center Operation Manager 代理程式應該安裝在 Windows Server Essentials 上，並設定與管理的信任使用憑證的伺服器。  
  
2.  下載管理組件。 若要使用 Operations Manager 2007 來監視 Windows Server Essentials，您必須先從管理元件類別目錄下載[Windows Server 作業系統管理元件](https://connect.microsoft.com/WindowsServer/Downloads/DownloadDetails.aspx?DownloadID=45010)。  
  
3.  匯入管理組件檔案。 如果您使用當地語系化的管理組件，則需要匯入主要管理組件檔案和語言套件。  
  
#### <a name="files-in-this-monitoring-pack"></a>監視組件中的檔案  
 適用于 Windows Server Essentials 的監視元件包含下列檔案：  
  
-   Microsoft.Windows.Server.2012.Essentials.mp  
  
-   < 地區設定\>的地區設定。  
  
### <a name="back-up-and-restore"></a>備份與還原  
 Windows Server Essentials 可讓您同時備份伺服器和用戶端。  
  
#### <a name="back-up-the-server"></a>備份伺服器  
 Windows Server Essentials 支援兩種備份伺服器的方式：內部部署備份和外部部署備份。  
  
 **內部部署備份**可讓您在個別磁碟上定期執行區塊層級的增量備份。 身為主機服務提供者，您可以將虛擬磁片連結至 Windows Server Essentials VM，並將伺服器備份設定為此虛擬磁片。 虛擬磁片應該位於與 Windows Server Essentials VM 不同的實體磁片上。  
  
- 如果您有另一種機制來備份 Windows Server Essentials VM，而您不想讓使用者看到 Windows Server Essentials 原生伺服器備份功能，您可以將其關閉，並從 Windows Server Essentials 移除所有相關的使用者介面。儀錶. 如需詳細資訊，請參閱[ADK 檔](https://go.microsoft.com/fwlink/p/?LinkID=249124)的自訂伺服器備份一節。  
  
  **外部部署備份**可讓您定期將伺服器資料備份至雲端服務。 您可以下載並安裝適用于 Windows Server Essentials 的 Microsoft Azure 備份整合模組，以利用 Microsoft 提供的 Azure 備份。  
  
  如果您或您的使用者偏好其他的雲端服務，則可執行以下步驟：  
  
1.  更新 Windows Server Essentials 儀表板的使用者介面，讓它提供您慣用雲端服務的連結，而不是預設的 Azure 備份。 如需詳細資訊，請參閱 [ADK 文件](https://go.microsoft.com/fwlink/p/?LinkID=249124)的＜自訂映像＞一節。  
  
2.  選擇性開發適用于 Windows Server Essentials 儀表板的增益集，以設定及管理雲端備份服務。  
  
#### <a name="back-up-the-client"></a>備份用戶端  
 Windows Server Essentials 支援兩種用戶端資料備份：完整用戶端備份和檔案歷程記錄。  
  
> [!NOTE]
>  由於資料必須透過 VPN 從用戶端傳送至伺服器，因此備份用戶端可能會對效能造成影響。  
  
 針對所有連線到 Windows Server Essentials 網路的用戶端裝置，預設會在上進行**完整的用戶端備份**。 它會以累加方式備份完整用戶端 (系統和資料) 並支援重複資料刪除。 備份資料將會在執行 Windows Server Essentials 的伺服器上。 失敗的用戶端會將資料還原至上一個備份點。 您可以遵循[ADK](https://technet.microsoft.com/library/jj200150)檔的建立 Cfg 檔案一節中的步驟來關閉這項功能。  
  
 以下是完整用戶端備份中的一些考量：  
  
- 效能：初始用戶端備份可能需要很多時間來上傳大量的資料。  
  
- 穩定性：有時候用戶端的網際網路連線可能會不穩定。 用戶端備份已設計為可繼續執行，且預設檢查點為 40 GB (每次在備份 40 GB 的資料後，用戶端備份資料庫都會建立一個檢查點)。 如果您認為網際網路連線不夠穩定，也可以將該值變更為較小的數字。  
  
  -   若要啟用檢查點工作，可在伺服器上將登錄機碼 HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs 設為 1。  
  
  -   若要變更檢查點閾值，可在用戶端上變更 HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold 的預設值 (40 GB)。  
  
- 用戶端裸機還原：由於 Windows 前置安裝環境不支援 VPN 連線，因此也無法支援用戶端裸機還原。  
  
  [檔案歷程**記錄**] 是將設定檔資料（媒體櫃、桌面、連絡人、我的最愛）備份到網路共用的 Windows 8.1 功能。 在 Windows Server Essentials 中，我們允許集中管理所有加入 Windows Server Essentials 的 Windows 8.1 用戶端的 [檔案歷程記錄] 設定。 備份資料會儲存在執行 Windows Server Essentials 的伺服器上。 您可以遵循[ADK](https://technet.microsoft.com/library/jj200150)檔的建立 Cfg 檔案一節中的步驟來關閉這項功能。  
  
### <a name="storage-management"></a>存放管理  
 [新的「儲存空間」功能](https://technet.microsoft.com/library/hh831739.aspx) 可讓您彙總不同硬碟的實體儲存容量、以動態方式新增硬碟，並以指定的彈性層級建立資料磁碟區。 您也可以將 iSCSI 磁片連結到 Windows Server Essentials 以擴充其存放裝置。  
  
## <a name="what-are-the-main-scenarios-i-should-test"></a>我應測試哪些主要實例？  
 從主控的觀點來看，我們建議您測試以下實例：  
  
 **伺服器部署**  
  
- 在您的實驗室環境中部署 Windows Server Essentials 伺服器。  
  
- 視需要自訂 Windows Server Essentials 映像。  
  
- 使用自動檔案和 cfg 來自動化 Windows Server Essentials 部署。  
  
- 將內部部署 Windows SBS 遷移至託管的 Windows Server Essentials。  
  
- 從 Windows Server Essentials 升級至 Windows Server 2012。  
  
  **伺服器設定**  
  
- 設定隨處存取 (VPN、遠端 Web 存取、DirectAccess)。  
  
- 設定儲存空間和伺服器資料夾。  
  
- (如果適用) 設定伺服器備份、線上備份、用戶端備份、檔案記錄。  
  
- (如果適用) 設定和管理儲存空間。  
  
- (如果適用) 設定電子郵件解決方案整合 (Office 365、Exchange 託管等等)。  
  
- (如果適用) 設定媒體伺服器。  
  
  **伺服器管理**  
  
- 管理使用者。  
  
- 設定及接收警示的電子郵件通知。  
  
- 在發生錯誤/警告時執行 BPA。  
  
- 設定 System Center 監視組件。  
  
- 設定損毀時的伺服器復原。  
  
  **用戶端體驗**  
  
- 透過網際網路的用戶端部署 (個人電腦或 Mac OS)。  
  
- 使用用戶端上的啟動列來存取共用資料夾。  
  
- 從不同裝置 (個人電腦、手機和平板電腦) 透過遠端 Web 存取來存取伺服器資產。  
  
- Windows Phone 的 My Server 應用程式。  
  
- (如果適用) 檔案記錄、用戶端備份和還原 (無 BMR)、資料夾重新導向。  
  
- (如果適用) 電子郵件整合經驗。  
  
## <a name="where-can-i-get-more-support"></a>如何取得支援？  
 您可以從以下連結取得 SDK 和 ADK 文件：  
  
- [SDK](https://go.microsoft.com/fwlink/p/?LinkID=248648)  
  
- [ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124)  
  
  您可以透過連線向功能小組報告錯誤。 如果要產生記錄檔，請壓縮伺服器和加入伺服器的用戶端上的下列資料夾：C:\ProgramData\Microsoft\Windows Server\Logs。
