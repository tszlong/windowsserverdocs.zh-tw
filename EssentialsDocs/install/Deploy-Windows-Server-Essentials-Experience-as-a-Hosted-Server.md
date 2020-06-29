---
title: 部署 Windows Server Essentials 體驗做為託管伺服器
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: a455c6b4-b29f-4f76-8c6b-1578b6537717
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5222298c4b8a3fd98c40474233fc7208607a3df7
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470993"
---
# <a name="deploy-windows-server-essentials-experience-as-a-hosted-server"></a>部署 Windows Server Essentials 體驗做為託管伺服器

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

本檔包含主控者的專屬資訊，其適用于想要部署 Windows server Essentials 體驗角色（在檔其餘部分中稱為「Windows Server Essentials」）的 Microsoft Windows Server 16，並打算以服務的形式提供給客戶的 Windows Server Essentials 體驗。 本文件包含下列各節：


-   [Windows Server Essentials 體驗總覽](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)

-   [裝載 Windows Server Essentials 體驗的優點](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)

-   [支援的部署選項](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)

-   [支援的網路拓撲](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)

-   [自訂 Windows Server Essentials 體驗角色的映像](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)

-   [自動部署 Windows Server Essentials 體驗](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)

-   [將資料從 Windows Small Business Server 移轉到 Windows Server Essentials 體驗](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)

-   [使用 Windows PowerShell 執行一般工作](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)

-   [整合電子郵件與 Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)

-   [使用原生工具監視和管理](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)

-   [測試情節](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)

-   [支援資訊](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)


##  <a name="windows-server-essentials-experience-overview"></a><a name="BKMK_WSEEOverview"></a>Windows Server Essentials 體驗總覽
 Windows Server Essentials 體驗是 Windows Server 2012 R2 Standard 和 Windows Server 2012 R2 Datacenter 中提供的伺服器角色。 當 Windows Server Essentials 體驗角色安裝在執行 Windows Server 2012 R2 的伺服器上時，客戶可以利用 Windows Server Essentials 中的所有功能，而不會有鎖定和限制。 Windows Server Essentials 體驗可為中小型企業提供下列跨單位的解決方案：

-   **資料儲存和保護**您可以在集中的位置儲存客戶的資料，並藉由備份網路內的伺服器和用戶端電腦（低於75）來保護伺服器和用戶端的資料。

-   **使用者管理** 您可以透過簡化的伺服器儀表板管理使用者及群組。 此外，與 Microsoft Azure Active Directory （Azure AD）整合，可讓使用者使用其網域認證輕鬆存取 Microsoft 線上服務（例如，Office 365、Exchange Online 和 SharePoint Online）的資料。

-   **服務整合**您可以整合伺服器與 Microsoft 線上服務（例如 Office 365、SharePoint Online 和 Microsoft Azure 備份）。 您也可以將伺服器與您的服務或由協力廠商提供者所提供的服務整合。

-   **隨處存取** 客戶可以從他們有網際網路連線的任何地點，以及使用幾乎任何裝置存取伺服器、網路電腦和資料。 遠端 Web 存取可讓客戶擁有簡化、友善的瀏覽器觸控體驗，存取應用程式和資料。 「我的伺服器」應用程式可讓他們從 Windows Phone 或 Microsoft Store 應用程式存取資料。

-   **媒體串流處理**如果您在已啟用 Windows Server Essentials 體驗的伺服器上安裝媒體套件，終端客戶可以將音樂、影片和相片儲存在共用資料夾中，然後從網路電腦或遠端 Web 存取存取這些媒體檔案。

-   **狀況監控** 您可以監視網路狀況，並取得自訂的健康情況報告。

##  <a name="benefits-of-hosting-windows-server-essentials-experience"></a><a name="BKMK_Benefits"></a>裝載 Windows Server Essentials 體驗的優點
  Windows Server Essentials 體驗是 Windows Server 中的一種角色，因此您可以重複使用 Windows Server 中現有的部署和管理架構，來部署和設定 Windows Server Essentials 體驗角色。 裝載 Windows Server Essentials 體驗角色可提供下列優點：

-   **簡化的部署**只要開啟 Windows Server Essentials 體驗角色，部分最常使用的角色和功能就會開啟，並以小型和中型企業的最佳做法進行設定。 您可以自訂 Windows Server Essentials 功能，或隱藏某些內部部署功能。 如果您使用 Windows Azure 套件，您可以下載 windows server 2012 R2 上 Windows Server Essentials 體驗的資源庫範本。

-   **簡化的儀表板** Windows Server Essentials 儀表板簡化一般工作，例如管理伺服器資料夾、伺服器儲存體、備份與還原、使用者或群組帳戶、裝置、遠端存取以及電子郵件。 小型和中型企業客戶可以執行每日的管理工作，而不用連絡技術支援人員。

-   **擴充性** Windows Server Essentials 儀表板和 Windows Server Essentials 連接器軟體都可以擴充。 您可以新增您自己的商標和服務整合，讓您的客戶從一個進入點就可以使用其伺服器和服務的所有項目。

-   **監視器** 新版的 System Center 監視組件可用來監視並管理多部執行 Windows Server Essentials 的伺服器。 若要下載管理元件，請參閱[適用于 Windows Server Essentials 的 System Center 管理元件](https://www.microsoft.com/download/details.aspx?id=40809)。

##  <a name="supported-deployment-options"></a><a name="BKMK_SupportedDeployment"></a>支援的部署選項
  Windows Server Essentials 體驗可以在新的 Active Directory 環境中部署為網域控制站;或者，它可以部署到現有的 Active Directory 環境做為網域成員。

 我們建議您先部署 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter，然後再安裝 Windows Server Essentials 體驗角色。 使用此部署方法，您可以取得 Windows Server Essentials 版本的所有功能，而不會有鎖定和限制。


 如需安裝 Windows server 2012 R2 與 Windows Server Essentials 體驗角色的詳細資訊，請參閱[安裝和設定 Windows Server essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。



##  <a name="supported-network-topologies"></a><a name="BKMK_SupportedToplogy"></a>支援的網路拓撲
 若要從漫遊用戶端使用 Windows Server Essentials 體驗，應啟用 VPN。 若要啟用從漫遊用戶端遠端存取伺服器，您必須開啟伺服器上的連接埠 443 和連接埠 80。

 以下是兩種一般的伺服器端網路拓撲，以及 VPN 和遠端 Web 存取的設定方法：

- **拓撲 1** (這是慣用的拓撲，它會將所有伺服器及 VPN IP 範圍都放在相同的子網路)：

  -   在網路位址轉譯 (NAT) 裝置下的個別虛擬網路中設定伺服器。

  -   啟用虛擬網路中的 DHCP 服務，或對伺服器指派靜態 IP 位址。

  -   轉寄路由器上的公用 IP 連接埠 443 到伺服器的本機網路位址。

  -   允許連接埠 443 的 VPN 通道。

  -   在與伺服器位址相同子網路的範圍中設定 VPN IPv4 位址集區。

  -   指派第二部伺服器相同子網路內的靜態 IP 位址，但位於 VPN 位址集區之外。

- **拓撲 2**：

  -   指派伺服器私人 IP 位址。

  -   允許伺服器的連接埠 443 到達公用連接埠 443 IP 位址。

  -   允許連接埠 443 的 VPN 通道。

  -   VPN IPv4 位址集區和伺服器位址指派不同的範圍。

  使用拓撲 2，不支援第二部伺服器案例，因為您無法將另一部伺服器加入相同網域。

  您可以使用 Windows PowerShell 指令碼在自動部署期間啟用 VPN，或者在初始設定之後使用精靈進行設定。

  若要使用 Windows PowerShell 啟用 VPN，請在執行 Windows Server Essentials 的伺服器上，以系統管理權限執行下列命令，並提供所有必要的資訊。

```
##
## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).
##

$myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server
$mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file
$mySslCertificatePassword = ConvertTo-SecureString  œAsPlainText  œForce '******';   ## password for private key of the SSL certificate
$skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate

Set-WssDomainNameConfiguration  œDomainName $myExternalDomainName  œCertificatePath $mySslCertificateFile  œCertificateFilePassword $mySslCertificatePassword  œNoCertificateVerification
##
## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).
##

Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;

```

> [!NOTE]
>  如果您在客戶擁有伺服器之前無法提供 VPN 連線，請確定伺服器連接埠 3389 可透過網際網路連線，以便客戶可以使用遠端桌面通訊協定來連線到伺服器並進行設定。

##  <a name="customize-the-image-of-windows-server-essentials-experience-role"></a><a name="BKMK_CustomizeImage"></a>自訂 Windows Server Essentials 體驗角色的映射
 您在設定 Windows Server Essentials 體驗角色之前可以自訂映像。 若要了解標準的 Windows Server Sysprep 程序，請參閱 [Windows 評定及部署套件](https://msdn.microsoft.com/library/hh825420.aspx)。 使用 Sysprep 準備映像之後，您可以使用它或封裝成 Install.wim 進行新的部署。

 如果您使用 Virtual Machine Manager，可以使用執行中的執行個體來建立範本。 此程序使用 Sysprep 準備執行個體，並將電腦關機。 將範本儲存在您的程式庫之後，您就可以依個別情況加以使用。

 安裝 Windows Server Essentials 體驗角色之後，您可以自訂 Windows Server Essentials 中的功能。 其中一個最重要的自訂項目是 **IsHosted** 登錄機碼： **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\Deployment\IsHosted**。

 如果這個機碼設定為 0x1，某些內部部署功能將會變更行為。 這些功能的變更包括：

- **用戶端備份** 新加入的用戶端電腦預設會關閉用戶端備份。

- **用戶端還原服務** 用戶端還原服務將會停用，儀表板也不會顯示 UI。

- **檔案歷程記錄** 伺服器不會自動管理新建立的使用者帳戶的檔案歷程記錄設定。

- **伺服器備份** 伺服器備份服務將會停用，儀表板也不會顯示伺服器備份 UI。

- **儲存空間** 儀表板不會顯示建立或管理儲存空間的 UI。

- **隨處存取** 當您執行「設定隨處存取精靈」時，預設將會略過路由器及 VPN 設定。

  如果您想要控制每個列出功能的行為，您可以針對每一項設定相對應的登錄機碼。 如需如何設定登錄機碼的相關資訊，請參閱 [在 Windows Server 2012 R2 中自訂和部署 Windows Server Essentials](https://technet.microsoft.com/library/dn293241.aspx)

##  <a name="automate-the-deployment-of-windows-server-essentials-experience"></a><a name="BKMK_AutomateDeployment"></a>自動部署 Windows Server Essentials 體驗
 若要自動化部署，您必須先部署作業系統，然後再安裝 Windows Server Essentials 體驗角色。

-   若要自動部署 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter，請遵循[Windows 評定及部署套件](https://msdn.microsoft.com/library/hh825420.aspx)中的指示。

-   若要瞭解如何使用 Windows PowerShell 安裝 Windows Server Essentials 體驗角色，請參閱[安裝和設定 Windows Server essentials](https://technet.microsoft.com/library/dn281793.aspx)。

> [!NOTE]
>  請確定主機虛擬機器和 Windows Server Essentials 體驗的時區設定相同。 否則，您可能會遇到幾個錯誤。 這些包括：伺服器的初始設定可能不會在憑證相關工作上成功，憑證可能會在安裝 Windows Server Essentials 體驗角色後的幾個小時內無法運作，且裝置資訊將無法正確更新。

 部署之後，請使用 Windows PowerShell Cmdlet **Get-WssConfigurationStatus** 確認初始設定是否成功。 傳回的狀態應為下列其中一項： **Notstarted**、 **FinishedWithWarning**、 **Running**、 **Finished**、 **Failed**或 **PendingReboot**。

 在初始設定期間，伺服器會重新啟動。 如果您需要避免自動重新啟動，在開始初始設定之前，您可以使用下列命令新增登錄機碼：

```
New-ItemProperty "HKLM:\Software\Microsoft\Windows Server\Setup"Ã‚Â  -Name "WaitForReboot" -Value 1 -PropertyType "DWord" -Force -Confirm:$false

```

 初始設定開始之後，您可以使用 **Get-WssConfigurationStatus** 檢查初始設定狀態，而當狀態為 **PendingReboot** 時，您可以重新啟動您的伺服器。

##  <a name="migrate-data-from-windows-small-business-server-to-windows-server-essentials-experience"></a><a name="BKMK_Migrate"></a>將資料從 Windows Small Business Server 遷移到 Windows Server Essentials 體驗
 您可以將資料從執行 Windows Small Business Server 2011、Windows Small Business Server 2008、Windows Small Business Server 2003 或 Windows Server Essentials 的伺服器遷移到執行 Windows Server Essentials 的伺服器。 請參閱[遷移至 Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)遷移指南以瞭解內部部署2個移轉，並根據您的裝載環境進行必要的自訂。

> [!NOTE]
>  我們建議您將來源伺服器和目的地伺服器放在相同的子網路。 如果此方法不可行，則應確定下列各項：
>
> - 來源伺服器和目的地伺服器可以存取彼此的「內部 DNS 名稱」。
>   -   所有必要的連接埠皆已開啟。

 移轉之後，您可以升級您的授權以移除鎖定與限制。 如需詳細資訊，請參閱[從 Windows Server Essentials 轉換到 Windows server 2012 Standard](https://technet.microsoft.com/library/jj247582.aspx)。

##  <a name="perform-common-tasks-by-using-windows-powershell"></a><a name="BKMK_PowerShell"></a>使用 Windows PowerShell 執行一般工作
 本節說明您可以使用 Windows PowerShell 執行的一些常見工作。

### <a name="enable-remote-web-access"></a>啟用遠端 Web 存取
 **語法**：

 Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]

 **範例**：

 $Enable Enable-wssremotewebaccess œDenyAccessByDefault œApplyToExistingUsers

 該命令會使用自動設定的路由器啟用遠端 Web 存取，並變更所有現有使用者的預設存取權限。

### <a name="add-user"></a>新增使用者
 **語法**：

 Add-wssuser [-Name] <string \> [-Password] <securestring \> [-AccessLevel <字串 \> {使用者 &#124; 系統管理員}] [-FirstName <字串 \> ] [-LastName <string \> ] [-AllowRemoteAccess] [-AllowVpnAccess] [<CommonParameters \> ]

 **範例**：

 $password = Convertto-html-SecureString "Passw0rd！" -asplaiNtext œforce $ Add-Add-wssuser-Name User2Test-Password $password-Accesslevel Administrator-FirstName：（LastName）測試

 此命令會新增名為 User2Test 且密碼為 Passw0rd！的系統管理員。

### <a name="add-server-folder"></a>新增伺服器資料夾
 **語法**：

 Add-wssfolder [-Name] <string \> [-Path] <string \> [[-Description] <string \> ] [-KeepPermissions] [<CommonParameters \> ]

 **範例**：

 $Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"

 此命令會在指定的位置新增名為 MyTestFolder 的伺服器資料夾。

##  <a name="email-integration-with-windows-server-essentials"></a><a name="BKMK_EmailIntegration"></a>與 Windows Server Essentials 整合的電子郵件
 您可以整合 Windows Server Essentials 體驗與 Office 365 或託管的 Exchange Server。 如果您希望客戶使用您裝載的電子郵件，您必須開發增益集將 Windows Server Essentials 體驗與裝載的電子郵件解決方案整合。 如需詳細資訊，請參閱 [Windows Server Essentials SDK](https://msdn.microsoft.com/library/gg513877.aspx)。

##  <a name="monitor-and-manage-by-using-native-tools"></a><a name="BKMK_Monitoring"></a>使用原生工具進行監視和管理
 本節討論 Windows Server 2012 R2 中可用來監視和管理伺服器的原生工具。

### <a name="group-policy"></a>群組原則
  Windows Server Essentials 體驗利用 Windows Server 2012 R2 中的原生群組原則支援，並提供使用者介面來設定資料夾重新導向和安全性設定。

> [!NOTE]
>  在裝載的環境中，如果使用者設定檔已啟用資料夾重新導向，當資料量過大時，使用者可能需要更多時間才能登入。

### <a name="system-center-monitoring-pack"></a>System Center 監視組件
 適用于 Windows Server Essentials 體驗的 System Center 監視元件會監視健康情況警示系統，協助您管理大量執行 Windows Server Essentials 的伺服器，專門用於小型企業組織。 如需詳細資訊，請參閱[適用于 Windows Server Essentials 的 System Center 管理元件](https://www.microsoft.com/download/details.aspx?id=40809)。

### <a name="backup-and-restore"></a>備份與還原
  Windows Server 2012 R2 with Windows Server Essentials 體驗可讓您備份網路中的伺服器和用戶端電腦。

#### <a name="server-backup"></a>伺服器備份
 Windows Server Essentials 支援兩種備份伺服器的方式：內部部署備份和外部部署備份。 如果您想要部署自己的伺服器備份解決方案，可以自訂這些選項。

-   **內部部署備份** 可讓您在個別磁碟上定期執行區塊層級的增量備份。 身為主機服務提供者，您可以將虛擬硬碟連接至執行 Windows Server Essentials 的虛擬機器，然後設定伺服器備份到此虛擬硬碟。 虛擬硬碟與執行 Windows Server Essentials 的虛擬機器應位於不同的實體磁碟上。

    > [!NOTE]
    >  如果您的虛擬機器有其他備份解決方案，而您不想讓使用者看見 Windows Server Essentials 原生的伺服器備份功能，您可以將它關閉，並從儀表板中移除相關的使用者介面。 如需詳細資訊，請參閱 [自訂和部署 Windows Server 2012 R2 中的 Windows Server Essentials](https://technet.microsoft.com/library/dn293413.aspx) 之 [自訂伺服器備份](https://technet.microsoft.com/library/dn293241.aspx)一節。

-   **外部部署備份** 可讓您定期將伺服器資料備份至雲端架構的服務。 您可以下載並安裝適用于 Windows Server Essentials 的 Microsoft Azure 備份整合模組，以利用 Microsoft 提供的 Azure 備份。

     如需詳細資訊，請參閱[管理伺服器備份](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md)中的整合 windows Azure 備份與 Windows server Essentials 一節。

     如果您或您的使用者偏好其他雲端服務，則可考慮下列動作：

    -   更新儀表板的使用者介面，讓它連結到您慣用的雲端服務，而不是預設的 Azure 備份。

    -   (選擇性) 開發適用於儀表板的增益集，以設定及管理雲端架構的備份服務。

#### <a name="client-computer-backup"></a>用戶端電腦備份
 Windows Server Essentials 體驗支援兩種用戶端資料備份：完整用戶端備份和檔案歷程記錄。

> [!NOTE]
>  由於資料必須透過 VPN 從用戶端傳送至伺服器，因此備份用戶端可能會對效能造成影響。

##### <a name="full-client-backup"></a>完整用戶端備份
 連線到 Windows Server Essentials 網路的所有用戶端裝置預設都會開啟完整用戶端備份。 它會完整備份用戶端的系統資訊和資料，並支援重複資料刪除。 備份資料會儲存在執行 Windows Server Essentials 的伺服器上。 這可讓失敗的用戶端從先前的備份點擷取資料。

 以下是完整用戶端備份的一些考量：

-   **效能**初始的用戶端備份可能需要花費很長的時間，因為要上傳的資料量。

-   **穩定性**有時候用戶端上的網際網路連線並不穩定。 用戶端備份設計為自動繼續進行，而且用戶端備份資料庫會在每次備份 40 GB 的資料時建立檢查點。 如果您認為網際網路連線不夠穩定，也可以將該值變更為較小的數字。

    -   若要啟用檢查點工作：在伺服器上，將登錄機碼**HKLM\Software\Microsoft\Windows server\backup\getcheckpointjobs 設定**設定為1。

    -   變更檢查點閾值：在用戶端上，變更 **HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold** 的預設值 40 GB。

-   **用戶端裸機還原** 由於 Windows 預先安裝環境不支援 VPN 連線，因此也不支援用戶端裸機還原。 您應遵循在[Windows server 2012 R2 中自訂和部署 Windows Server Essentials](https://technet.microsoft.com/library/dn293241.aspx)中的步驟來隱藏用戶端還原服務工作。

##### <a name="file-history"></a>檔案歷程記錄
 檔案歷程記錄是 Windows 8.1 和 Windows 8 中將設定檔資料 (媒體櫃、桌面、連絡人和我的最愛) 備份到網路共用的功能。 您可以為所有執行 Windows 8.1 或 Windows 8 且加入 Windows Server Essentials 網路的電腦，集中管理檔案歷程記錄設定。 備份資料會儲存在執行 Windows Server Essentials 的伺服器上。 您必須遵循 [自訂和部署 Windows Server 2012 R2 中的 Windows Server Essentials](https://technet.microsoft.com/library/dn293241.aspx)之步驟來隱藏用戶端還原服務工作。

### <a name="storage-management"></a>存放管理
 「儲存空間」可讓您彙總不同硬碟的實體儲存空間、以動態方式新增硬碟，並以指定的彈性層級建立資料磁碟區。 您可以在主機或虛擬機器上，執行這項操作。 如果您想要在執行 Windows Server Essentials 的虛擬機器中隱藏這項功能，請遵循 [自訂和部署 Windows Server 2012 R2 中的 Windows Server Essentials](https://technet.microsoft.com/library/dn293241.aspx)之指示。

##  <a name="test-scenarios"></a><a name="BKMK_Scenarios"></a>測試案例
 從裝載的觀點來看，我們建議您測試下列案例：


-   [伺服器部署](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerDeploy)

-   [伺服器設定](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerConfig2)

-   [伺服器管理](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerManage)

-   [用戶端體驗](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ClientXP)


###  <a name="server-deployment"></a><a name="BKMK_ServerDeploy"></a>伺服器部署
 您可以測試下列伺服器部署案例：

-   在您的實驗室環境中，將執行 Windows Server 2012 R2 的伺服器部署為網域控制站，然後安裝 Windows Server Essentials 體驗角色。

-   在您的實驗室環境中部署執行 Windows Server 2012 R2 的伺服器，將此伺服器加入現有的網域，然後安裝 Windows Server Essentials 體驗角色。

-   視需要自訂 Windows Server Essentials 映像。

-   使用自動檔與 Windows PowerShell 自動部署 Windows Server Essentials。

-   移轉執行 Windows Small Business Server 的內部部署伺服器到執行 Windows Server Essentials 的裝載伺服器。

###  <a name="server-configuration"></a><a name="BKMK_ServerConfig2"></a>伺服器設定
 您可以測試下列伺服器設定案例：

-   設定隨處存取 (虛擬私人網路、遠端 Web 存取以及 DirectAccess)。

-   設定儲存空間和伺服器資料夾。

-   開啟 BranchCache。

-   (如果適用) 設定伺服器備份、線上備份、用戶端備份以及檔案歷程記錄。

-   (如果適用) 設定和管理儲存空間。

-   (如果適用) 設定電子郵件解決方案整合 (Office 365 和裝載的 Exchange Server)。

-   (如果適用) 設定與其他 Microsoft 線上服務的整合。

-   (如果適用) 設定媒體伺服器。

###  <a name="server-management"></a><a name="BKMK_ServerManage"></a>伺服器管理
 您可以測試下列伺服器管理案例：

-   管理使用者和群組。

-   設定及接收警示的電子郵件通知。

-   執行最佳做法分析程式，以查看是否有錯誤或警告訊息。

-   設定 System Center Operations Manager 組件。

-   設定作業系統中發生損毀時的伺服器復原。

###  <a name="client-experience"></a><a name="BKMK_ClientXP"></a>用戶端體驗
 您可以測試下列一般使用者案例：

-   透過網際網路部署用戶端 (PC 或 Mac 作業系統)。

-   使用用戶端上的啟動列存取共用資料夾。

-   從不同裝置 (PC、手機和平板電腦) 透過遠端 Web 存取來存取伺服器資產。

-   存取 Windows Phone 的 My Server 應用程式。

-   (如果適用) 存取檔案歷程記錄、用戶端備份與還原以及資料夾重新導向。

-   (如果適用) 確認電子郵件整合體驗。

##  <a name="support-information"></a><a name="BKMK_Support"></a>支援資訊
 您可以下載 Windows Server Essentials 軟體發展工具組（SDK）和 Windows Server Essentials 評定及部署套件（ADK）：

-   [Windows Server Essentials 軟體發展工具組](https://msdn.microsoft.com/library/gg513877.aspx)SDK

-   [在 Windows Server 2012 R2 中自訂和部署 Windows Server Essentials](https://technet.microsoft.com/library/dn293241.aspx)

## <a name="additional-references"></a>其他參考

-   [Windows Server Essentials 中的新功能](../get-started/what-s-new.md)

-   [安裝 Windows Server Essentials](Install-Windows-Server-Essentials.md)

-   [開始使用 Windows Server Essentials](../get-started/get-started.md)
