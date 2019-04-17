---
title: "Windows Server Essentials 體驗部署為裝載伺服器"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a455c6b4-b29f-4f76-8c6b-1578b6537717
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c299d2b5f3d6b48693c473754a5205a7d26b5d6a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-windows-server-essentials-experience-as-a-hosted-server"></a>Windows Server Essentials 體驗部署為裝載伺服器

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

本文件包含主機人想要部署 Microsoft Windows Server 16 與 Windows Server Essentials 體驗角色（稱為 Windows Server Essentials 的文件中的其餘部分）安裝它們 lab 中，並想要即服務提供其針對 Windows Server Essentials 體驗服務提供者的特定的資訊。 本文件包含下列的區段：  
  

-   [Windows Server Essentials 體驗概觀](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [管理 Windows Server Essentials 體驗的優點](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [部署支援的選項](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [支援的網路拓撲](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [自訂 Windows Server Essentials 體驗角色的影像](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [自動部署的 Windows Server Essentials 的體驗](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [從 Windows 小型企業 Server 的資料移轉 Windows Server Essentials 體驗](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [使用 Windows PowerShell 來執行一般工作](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [電子郵件與 Windows Server Essentials 的整合](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [監視和管理使用原生工具](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [測試案例](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [支援的資訊](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

-   [Windows Server Essentials 體驗概觀](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [管理 Windows Server Essentials 體驗的優點](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [部署支援的選項](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [支援的網路拓撲](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [自訂 Windows Server Essentials 體驗角色的影像](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [自動部署的 Windows Server Essentials 的體驗](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [從 Windows 小型企業 Server 的資料移轉 Windows Server Essentials 體驗](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [使用 Windows PowerShell 來執行一般工作](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [電子郵件與 Windows Server Essentials 的整合](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [監視和管理使用原生工具](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [測試案例](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [支援的資訊](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

  
##  <a name="BKMK_WSEEOverview"></a>Windows Server Essentials 體驗概觀  
 Windows Server Essentials 體驗是可在 Windows Server 2012 標準 R2 和 Windows Server 2012 R2 Datacenter 伺服器角色。 Windows Server Essentials 體驗的角色是執行 Windows Server 2012 R2 的伺服器上安裝客戶可以利用的所有功能，可在 Windows Server Essentials 的鎖定和限制。 Windows Server Essentials 體驗可讓您的中型小型企業下列跨先方案：  
  
-   **資料儲存空間及保護**您可以將客戶」度 s 集中式的位置資料，並保護 server 和 client 資料備份 client 電腦 (小於 75) 從網路與伺服器。  
  
-   **管理使用者**您可以管理使用者和群組透過簡化的伺服器儀表板。 此外，integration 的 Microsoft Azure Active Directory (Azure AD) 可讓 Microsoft online 服務（例如 Office 365、Online 換貨或 SharePoint Online）的資料輕鬆存取的使用者使用它們 domain 認證。  
  
-   **服務整合**整合（例如 Office 365、SharePoint Online 和 Microsoft Azure 備份）的 Microsoft online services 的伺服器。 您也可以將伺服器整合服務或協力廠商提供者所提供的服務。  
  
-   **隨處存取**客戶可以存取伺服器、電腦，以及資料從幾乎任何地方具有網際網路連接並使用幾乎所有裝置。 遠端網路存取可讓存取應用程式與簡化、方便觸控的瀏覽器的經驗資料它們。 我伺服器應用程式可讓他們從 Windows Phone 或 Microsoft 網上商店應用程式存取的資料。  
  
-   **串流媒體**如果您支援 Windows Server Essentials 體驗的伺服器上安裝媒體套件，結束客戶可以共用資料夾中儲存的音樂、影片、和相片，然後從網路的電腦或存取網路存取這些媒體檔案。  
  
-   **健康監視**您可以監視網路健康狀態，並取得健康自訂的報告。  
  
##  <a name="BKMK_Benefits"></a>管理 Windows Server Essentials 體驗的優點  
  Windows Server Essentials 體驗是在 Windows Server 的角色，您可以重新使用現有的部署及管理架構在 Windows Server 部署，並設定「Windows Server Essentials 體驗角色。 管理 Windows Server Essentials 體驗角色提供下列優點：  
  
-   **簡化部署**，只要將在 Windows Server Essentials 體驗的角色，一些最常使用的角色及功能的電腦設定的最佳做法根據小型企業。 您可以自訂 Windows Server Essentials 的功能，或隱藏場所上功能的部分。 如果您使用 Windows Azure 套件，您可以下載「影像中心」範本 Windows Server 2012 R2 上的 Windows Server Essentials 體驗。  
  
-   **簡化儀表板**Windows Server Essentials 儀表板簡化管理伺服器資料夾、伺服器儲存空間、備份與還原、使用者或群組帳號，裝置、遠端存取和電子郵件，例如一般工作。 小，並根據商務針對可以執行日常管理工作，而不是工程師通話的技術支援。  
  
-   **擴充性**的最具擴充性的 Windows Server Essentials 儀表板及 Windows Server Essentials 連接器軟體。 您可以新增自己的版本，並服務整合，以便您針對項目點的所有項目有關於其伺服器與服務。  
  
-   **監視器**系統中心監視套件的新版本可供監視和管理多部執行 Windows Server Essentials 的伺服器。 若要下載管理組件，請查看[系統中心管理組件的 Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809)。  
  
##  <a name="BKMK_SupportedDeployment"></a>部署支援的選項  
  Windows Server Essentials 的體驗，可以為網域控制站在新的 Active Directory 環境; 部署或在現有的 Active Directory 環境網域成員可以部署。  
  
 我們建議您先部署 Windows Server 2012 標準 R2 或 Windows Server 2012 R2 Datacenter 並再重新安裝 Windows Server Essentials 體驗角色。 使用此部署方法，您收到 Windows Server Essentials 的版本，鎖定而限制的所有的功能。  
  

 如需與 Windows Server Essentials 體驗角色安裝 Windows Server 2012 R2 的相關資訊，請查看[安裝與設定 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。  


  
##  <a name="BKMK_SupportedToplogy"></a>支援的網路拓撲  
 若要從漫遊 client 使用 Windows Server Essentials 的體驗，您應該支援 VPN。 若要讓漫遊從遠端存取伺服器，您需要開放 443 連接埠和連接埠 80 伺服器上。  
  
 以下兩種一般伺服器端網路拓撲，並無法如何設定遠端 Web Access 與 VPN:  
  
-   **1 拓撲**（這是慣用的拓撲，且所有伺服器和 VPN IP 範圍都放在同一個子網路中。）：  
  
    -   設定 virtual 網路位址轉譯 (NAT) 的裝置在不同網路中的伺服器。  
  
    -   讓 DHCP 服務 virtual 網路，或指定的伺服器靜態 IP 位址。  
  
    -   向前公用 IP 的連接埠 443 伺服器的區域網路位址路由器上。  
  
    -   允許 VPN 傳遞 443 連接埠。  
  
    -   在同一個子網路範圍伺服器的位址與設定的 VPN IPv4 位址集區。  
  
    -   指派第二個伺服器靜態 IP 位址子網路相同，但退出 VPN 位址集區。  
  
-   **拓撲 2**:  
  
    -   指派伺服器私人 IP 位址。  
  
    -   連接埠 443 允許伺服器瑞曲之戰公用連接埠 443 IP 位址。  
  
    -   允許 VPN 傳遞 443 連接埠。  
  
    -   指派不同範圍 VPN IPv4 位址集區與伺服器的位址。  
  
 使用拓撲 2 第二個伺服器案例不支援，因為您無法以相同的網域新增另一部伺服器。  
  
 您可以使用我們的 Windows PowerShell 指令碼，在自動部署讓 VPN，或者也精靈中設定的初始設定後。  
  
 使用 Windows PowerShell 來讓 VPN、執行 Windows Server Essentials 的伺服器上，使用系統管理員權限執行下列命令，並提供所需的所有資訊。  
  
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
>  如果您無法提供 VPN 連接擁有伺服器客戶之前，，確定伺服器連接埠 3389 已連接到網際網路，以便客戶可以使用遠端桌面通訊協定連接到伺服器，並將其設定。  
  
##  <a name="BKMK_CustomizeImage"></a>自訂 Windows Server Essentials 體驗角色的影像  
 您可以自訂映像，才能設定 Windows Server Essentials 體驗角色。 若要深入了解標準 Windows Server Sysprep 程序，請查看[Windows 評定及部署套件](https://msdn.microsoft.com/library/hh825420.aspx)。 您使用 Sysprep 準備映像之後，您可以使用，或重新新部署的到 Install.wim 封裝。  
  
 如果您使用管理員一樣，您可以建立範本利用執行個體。 此程序使用 Sysprep 準備執行個體，並將電腦關機。 您儲存在您的媒體櫃中的範本之後，您可以使用它來案例為基礎。  
  
 安裝 Windows Server Essentials 體驗角色之後，您可以自訂 Windows Server Essentials 的功能。 其中一個最重要的自訂項目是**IsHosted**鍵登錄：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\Deployment\IsHosted**。  
  
 如果 0x1 來設定此機碼，前提上功能的部分將會變更行為。 這些功能變更包括：  
  
-   **Client 備份**Client 的備份將會關閉藉由新加入的 client 電腦的預設值。  
  
-   **Client 還原服務**Client 還原服務將會停用，且 UI 將會隱藏起來從儀表板。  
  
-   **檔案歷史**帳號新建立的使用者設定檔歷史將不會自動管理的伺服器。  
  
-   **伺服器備份**伺服器備份服務會停用，並伺服器備份 UI 儀表板中將會隱藏起來。  
  
-   **儲存空間**UI 建立，或管理儲存空間的儀表板將會隱藏起來。  
  
-   **隨處存取**當您執行設定任何地方存取精靈，將會預設略路由器及 VPN 設定。  
  
 如果您想要控制列出每項功能，您可以針對每個設定對應登錄鍵。 如何設定登錄金鑰的相關資訊，請參考[自訂及部署 Windows Server Essentials 在 Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
##  <a name="BKMK_AutomateDeployment"></a>自動部署的 Windows Server Essentials 的體驗  
 若要自動部署，您需要先部署作業系統，然後再安裝 Windows Server Essentials 體驗角色。  
  
-   若要自動將 Windows Server 2012 標準 R2 或 Windows Server 2012 R2 資料中心部署，請遵循[Windows 評定及部署套件](https://msdn.microsoft.com/library/hh825420.aspx)。  
  
-   若要了解如何使用 Windows PowerShell 來安裝 Windows Server Essentials 體驗的角色，請查看[安裝與設定 Windows Server Essentials](https://technet.microsoft.com/library/dn281793.aspx)。  
  
> [!NOTE]
>  確保主機一樣和 Windows Server Essentials 體驗的時區設定相同。 否則，您可能會遇到幾個錯誤。 其中包括：伺服器的初次設定可能不會在成功憑證相關的工作、憑證可能不適用於已安裝 Windows Server Essentials 體驗的角色，以及裝置資訊不會正常更新後的幾個小時。  
  
 部署之後，使用 Windows PowerShell cmdlet**取得-WssConfigurationStatus**若要確認是否成功的初始設定。 傳回的狀態應該下列其中一個動作：**Notstarted**，**FinishedWithWarning**、**執行**、**已經完成**、**失敗**，或**PendingReboot**。  
  
 在初始設定時，將會重新啟動伺服器。 如果您要避免這個自動重新開機，您可以使用下列命令新增登錄按鍵初始設定您在開始之前：  
  
```  
New-ItemProperty "HKLM:\Software\Microsoft\Windows Server\Setup"Ã‚Â  -Name "WaitForReboot" -Value 1 -PropertyType "DWord" -Force -Confirm:$false  
  
```  
  
 開始初始設定後，您可以使用**取得-WssConfigurationStatus**若要查看的初始設定狀態，並狀態時**PendingReboot**，您可以重新開始您的伺服器。  
  
##  <a name="BKMK_Migrate"></a>從 Windows 小型企業 Server 的資料移轉 Windows Server Essentials 體驗  
 您可以從在執行 Windows Server Essentials 的伺服器來執行 Windows 小型企業 Server 2011、Windows 小型企業 Server 2008、Windows 小型企業 Server 2003 或 Windows Server Essentials 的伺服器移轉的資料。 檢視[Windows Server essentials 移轉](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)移轉的先 2migrations，指南，請根據您的主機環境必要的自訂項目。  
  
> [!NOTE]
>  我們建議您將來源伺服器和目的地伺服器放在同一個子網路中。 如果這不是可能，您應該請確認：  
>   
>  -   來源伺服器和目的地伺服器可以存取其他」度 s 內部 DNS 名稱。  
> -   必要的連接埠的開放。  
  
 移轉之後，您可以升級您的授權移除鎖定和限制。 如需詳細資訊，請查看[Windows Server Essentials 從轉換到 Windows Server 2012 標準](https://technet.microsoft.com/library/jj247582.aspx)。  
  
##  <a name="BKMK_PowerShell"></a>使用 Windows PowerShell 來執行一般工作  
 本章節解釋一些您可以使用 Windows PowerShell 來執行一般工作。  
  
### <a name="enable-remote-web-access"></a>可讓遠端存取  
 **語法**:  
  
 Enable-WssRemoteWebAccess[-SkipRouter][-DenyAccessByDefault][-ApplyToExistingUsers]  
  
 **範例**:  
  
 $Enable-WssRemoteWebAccess œDenyAccessByDefault œApplyToExistingUsers  
  
 這個命令將讓遠端 Web Access 與路由器會自動設定，並變更所有現有的使用者的預設的存取權限。  
  
### <a name="add-user"></a>將使用者新增  
 **語法**:  
  
 Add-WssUser[-名稱] < string\ > [-密碼] < securestring\ > [-AccessLevel < string\ > {使用者和 #124;系統管理員}] [-FirstName < string\ >] [-姓氏 < string\ >] [-AllowRemoteAccess] [-AllowVpnAccess] < [CommonParameters\ >]  
  
 **範例**:  
  
 $password = ConvertTo-SecureString」Passw0rd」！ -asplaintext œforce$ 新增-WssUser-名稱 User2Test-密碼 $password Accesslevel 系統管理員-FirstName User2-姓氏測試  
  
 這個命令，將會新增系統管理員密碼 Passw0rd 名 User2Test。  
  
### <a name="add-server-folder"></a>新增資料夾伺服器  
 **語法**:  
  
 Add-WssFolder[-名稱] < string\ > [-Path] < string\ > [[-描述] < string\ >] [-KeepPermissions] < [CommonParameters\ >]  
  
 **範例**:  
  
 $新增 WssFolder-名稱」MyTestFolder」-Path」C:\ServerFolders\MyTestFolder」  
  
 這個命令，將會新增伺服器名 MyTestFolder 在指定的位置。  
  
##  <a name="BKMK_EmailIntegration"></a>電子郵件與 Windows Server Essentials 的整合  
 您可以使用 Office 365 整合 Windows Server Essentials 體驗或裝載 Exchange Server。 如果您希望您使用電子郵件裝載的客戶，您需要開發增益集整合裝載的電子郵件方案 Windows Server Essentials 的體驗。 如需詳細資訊，請查看[Windows Server Essentials SDK](https://msdn.microsoft.com/library/gg513877.aspx)。  
  
##  <a name="BKMK_Monitoring"></a>監視和管理使用原生工具  
 本節適用於「Windows Server 2012 R2 監視和管理伺服器的原生工具。  
  
### <a name="group-policy"></a>群組原則  
  Windows Server Essentials 體驗運用 Windows Server 2012 R2 的原生支援群組原則，並提供使用者介面設定資料夾重新導向和安全性設定。  
  
> [!NOTE]
>  在裝載環境中，如果支援資料夾重新導向的使用者設定檔，它可以增加使用者登入資料大小時大的時間。  
  
### <a name="system-center-monitoring-pack"></a>System Center 監視套件  
 Windows Server Essentials 體驗監視器可協助您管理執行 Windows Server Essentials 的伺服器大量健康警示系統致力於小型公司組織 system Center 監視封包。 如需詳細資訊，請查看[系統中心管理組件的 Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809)。  
  
### <a name="backup-and-restore"></a>備份與還原  
  Windows Server 2012 R2 的 Windows Server Essentials 體驗可讓您備份伺服器及 client 網路中的電腦。  
  
#### <a name="server-backup"></a>伺服器備份  
 Windows Server Essentials 支援兩種方式可備份伺服器：先備份與關閉場所備份。 如果您想要部署自己伺服器備份方案，您可以自訂這些選項。  
  
-   **先備份**可讓您以不同的磁碟定期執行封鎖層級增量備份。 項，以可以執行 Windows Server Essentials，一樣連接 virtual 硬碟，然後設定伺服器備份到此 virtual 硬碟。 應該位於 virtual 硬碟比一樣執行 Windows Server Essentials 的其他實體磁碟上。  
  
    > [!NOTE]
    >  如果您的電腦虛擬有其他備份方案，並不想讓使用者看到的 Windows Server Essentials 原生伺服器備份功能，您可以將它關閉和移除儀表板中相關的使用者介面。 如需詳細資訊，請查看[自訂伺服器備份](https://technet.microsoft.com/library/dn293413.aspx)區段[自訂及部署 Windows Server Essentials Windows Server 2012 R2 在](https://technet.microsoft.com/library/dn293241.aspx)。  
  
-   **關閉場所備份**可讓您可以定期備份您 server 的資料，以雲端為基礎的服務。 您可以下載並安裝 Microsoft Azure 備份整合模組適用於 Windows Server Essentials 利用由 Microsoft Azure 備份。  
  
     如需詳細資訊，請查看整合 Windows Azure 備份與 Windows Server Essentials 一節中[管理伺服器備份](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md)。  
  
     如果您或您的使用者想要另一個雲端服務，您應該下列動作：  
  
    -   更新儀表板的使用者介面讓它連結預設 Azure 備份而不是您慣用的雲端服務。  
  
    -   （選擇性）開發儀表板設定及管理雲端為基礎的服務備份增益集。  
  
#### <a name="client-computer-backup"></a>Client 電腦備份  
 Windows Server Essentials 體驗支援兩種 client 資料備份：完整備份 client 和歷史檔案。  
  
> [!NOTE]
>  備份 client 可能會影響的效能，因為資料必須 client 的伺服器來傳送透過 VPN。  
  
##### <a name="full-client-backup"></a>完整 client 備份  
 根據預設，所有 client 裝置連接到 Windows Server Essentials 網路位於 client 完整備份。 完整備份資料與系統資訊 client，並支援 deduplication 資料。 備份資料將會儲存在執行 Windows Server Essentials 的伺服器上。 這可讓失敗的 client 從先前的備份點擷取的資料。  
  
 適用於電腦的完整 client 備份事項︰  
  
-   **效能**上傳到的資料量而初始 client 備份可能會花時間。  
  
-   **穩定性**有時候不穩定 client 一邊連接網際網路。 Client 備份設計用來自動，繼續與 client 備份資料庫會建立檢查點每一次備份時 40 GB 的資料。 如果您希望是不可靠連接網際網路，您可以變更此值為較小的數字。  
  
    -   檢查點工作，以便：在伺服器上，設定登錄鍵**HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs** 1。  
  
    -   若要變更檢查點臨界值：client，在 [變更**HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold**與 40 GB 的預設值。  
  
-   **Client 枯金屬還原**Windows 預先安裝環境不支援 VPN 連接，因為 client 枯金屬還原不受支援。 您應該依照下列步驟來隱藏 Client 還原服務工作[自訂及部署 Windows Server Essentials Windows Server 2012 R2 在](https://technet.microsoft.com/library/dn293241.aspx)。  
  
##### <a name="file-history"></a>歷史檔案  
 檔案歷史網路共用備份（媒體櫃、桌面、連絡人、[我的最愛]）的個人檔案資料是 Windows 8.1 和 Windows 8 中的功能。 您可以集中管理所有的電腦執行的是 Windows 8.1 或 Windows 8 的 Windows Server Essentials 的網路所加入的檔案歷史設定。 執行 Windows Server Essentials 的伺服器上儲存的備份資料。 您應該依照下列步驟來隱藏 Client 還原服務工作[自訂及部署 Windows Server Essentials 在 Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
### <a name="storage-management"></a>管理儲存空間  
 儲存空間可讓您所在的儲存空間容量的不同的硬碟機彙總、動態新增磁碟機和建立資料磁碟區，彈性的指定層級。 您可以在主機上或一樣。 如果您想要隱藏在執行 Windows Server Essentials 一樣這項功能，請依照下列指示[自訂及部署 Windows Server Essentials Windows Server 2012 R2 在](https://technet.microsoft.com/library/dn293241.aspx)。  
  
##  <a name="BKMK_Scenarios"></a>測試案例  
 控管觀點，我們建議您測試下列案例：  
  

-   [伺服器部署](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerDeploy)  
  
-   [伺服器設定](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerConfig2)  
  
-   [伺服器管理](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerManage)  
  
-   [Client 的體驗](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ClientXP)  

  
###  <a name="BKMK_ServerDeploy"></a>伺服器部署  
 您可以測試下列伺服器部署案例：  
  
-   部署伺服器執行 Windows Server 2012 R2 網域控制站在您 lab 環境中，再重新安裝 Windows Server Essentials 體驗角色。  
  
-   部署 lab 環境中執行 Windows Server 2012 R2 的伺服器，此伺服器加入現有的網域，並再重新安裝 Windows Server Essentials 體驗角色。  
  
-   視需要來自訂 Windows Server Essentials 映像。  
  
-   自動自動的檔案及 Windows PowerShell 與 Windows Server Essentials 部署。  
  
-   移轉到執行 Windows Server Essentials 裝載伺服器執行 Windows 的小型企業伺服器上場所伺服器。  
  
###  <a name="BKMK_ServerConfig2"></a>伺服器設定  
 您可以測試下列伺服器組態案例：  
  
-   設定隨處存取（virtual 私人網路，網路存取和 DirectAccess）。  
  
-   將設定儲存空間」和「伺服器資料夾。  
  
-   打開 BranchCache。  
  
-   （如果適用）伺服器備份、Online Backup Client 的備份設定和檔案歷史。  
  
-   （如果適用）設定及管理儲存空間。  
  
-   （如果適用）設定電子郵件方案整合（Office 365 和裝載的 Exchange Server）。  
  
-   （如果適用）其他 Microsoft online services 的設定整合。  
  
-   （如果適用）設定媒體伺服器。  
  
###  <a name="BKMK_ServerManage"></a>伺服器管理  
 您可以測試下列伺服器管理案例：  
  
-   管理使用者和群組。  
  
-   設定並通知的警示的電子郵件。  
  
-   執行最佳做法分析，以查看是否有錯誤或警告訊息。  
  
-   設定 System Center Operations Manager 封包。  
  
-   萬一損壞作業系統中設定伺服器復原。  
  
###  <a name="BKMK_ClientXP"></a>Client 的體驗  
 您可以測試下列使用者案例：  
  
-   部署戶端透過網際網路（PC 或 Mac 作業系統）。  
  
-   使用上 client Launchpad 存取共用資料夾。  
  
-   存取伺服器資產遠端 Web 存取透過從其他裝置（電腦、手機及平板電腦）。  
  
-   適用於 Windows Phone 存取我伺服器的 app。  
  
-   （如果適用）存取檔案歷史、Client 備份與還原及資料夾重新導向。  
  
-   （如果適用）請確認電子郵件整合體驗。  
  
##  <a name="BKMK_Support"></a>支援的資訊  
 Windows Server Essentials 軟體開發套件 (SDK) 與 Windows Server Essentials 評定及部署套件 (ADK)，您可以下載：  
  
-   [Windows Server Essentials 軟體開發套件](https://msdn.microsoft.com/library/gg513877.aspx)SDK  
  
-   [來自訂和部署 Windows Server 2012 R2 的 Windows Server Essentials](https://technet.microsoft.com/library/dn293241.aspx)  
  
## <a name="see-also"></a>也了  
  
-   [Windows Server Essentials 中的新功能](../get-started/what-s-new.md)  

-   [安裝 Windows Server Essentials](Install-Windows-Server-Essentials.md)  

-   [開始使用 Windows Server Essentials](../get-started/get-started.md)
