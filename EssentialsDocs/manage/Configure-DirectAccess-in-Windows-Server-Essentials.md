---
title: "在 Windows Server Essentials 設定 DirectAccess"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c959b6fc-c67e-46cd-a9cb-cee71a42fa4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cc336dcd2a5418aa79254108c941a02147112e8f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="configure-directaccess-in-windows-server-essentials"></a>在 Windows Server Essentials 設定 DirectAccess

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

本主題提供逐步指示設定 Windows Server Essentials DirectAccess，可讓您的行動裝置版工作人員順暢連接到您的組織的網路從網際網路配備遠端位置而連接私人網路 virtual (VPN)。 DirectAccess 可以提供行動裝置版的背景工作相同連接的體驗中及 office 外從他們 Windows 8.1、 Windows 8 和 Windows 7 的電腦。  
  
 在 Windows Server Essentials，如果網域中包含一部以上的 Windows Server Essentials 伺服器，DirectAccess 必須設定的網域控制站。  
  
> [!NOTE]
>  本主題提供適用於設定 DirectAccess，當您的 Windows Server Essentials 伺服器的網域控制站的指示。 如果 Windows Server Essentials 伺服器網域成員，請遵循指示來設定的網域成員 DirectAccess[新增的 DirectAccess 現有遠端存取 (VPN) 部署到](https://technet.microsoft.com/library/jj574220.aspx)改為。  
  
## <a name="process-overview"></a>程序概觀  
 若要在 Windows Server Essentials 設定 DirectAccess，請完成下列步驟。  
  
> [!IMPORTANT]
>  本指南使用的程序在 Windows Server Essentials 設定 DirectAccess 之前，您必須讓 VPN 伺服器上。 適用於的指示，請查看[管理 VPN](Manage-VPN-in-Windows-Server-Essentials.md)。  
  
-   [步驟 1： 新增到您 server 的遠端存取管理工具](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddRAM)  
  
-   [步驟 2： 靜態 IP 位址變更網路介面卡伺服器的位址](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddStaticIP)  
  
-   [步驟 3： 準備網路位置伺服器的憑證和 DNS 記錄](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)  
  
    -   [步驟 3a： 完整存取權限授 Authenticated Users 與 Web 伺服器的憑證範本](#BKMK_GrantFullPermissions)  
  
    -   [步驟 3b： 註冊一般的名稱解析外部網路，網路位置伺服器的憑證](#BKMK_EnrollaCertificate)  
  
    -   [步驟 3 c： 新增新的主機上的 DNS 伺服器，並將它對應的 Windows Server Essentials 伺服器地址](#BKMK_MapNewHosttoServerAddress)  
  
-   [步驟 4： 建立 DirectAccess client 電腦安全性群組](#BKMK_AddSecurityGroup)  
  
-   [步驟 5： 讓和設定 DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableConfigureDA)  
  
    -   [步驟 5a: DirectAccess 可以使用遠端存取管理主控台](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
    -   [步驟 5b： 移除無效 IPv6Prefix RRAS GPO (僅限 Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
    -   [步驟 5 c： 讓 client 電腦執行的 Windows 7 企業版使用 DirectAccess](#BKMK_Step4cWindows7Setup)  
  
    -   [步驟 5 d︰ 將網路位置伺服器設定](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
    -   [步驟 5e： 新增略過 CA 憑證，當您建立 IPsec 頻道登錄按鍵](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
-   [步驟 6： 設定 DirectAccess 伺服器的名稱解析原則的資料表設定](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NRPT)  
  
-   [步驟 7： 設定 DirectAccess 伺服器 Gpo 的 TCP 與 UDP 免](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_TCPUDP)  
  
-   [若要聆聽 IP-HTTPS 介面執行 「 步驟 8: DNS64 變更設定](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS64)  
  
-   [步驟 9： 保留 WinNat 服務連接埠](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_ExemptPort)  
  
-   [步驟 10： 重新開機 WinNat 服務](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_WinNAT)  
  
> [!NOTE]
>  [附錄： 使用 Windows PowerShell 來 DirectAccess 設定](#BKMK_AppendixBPowerShellScript)提供執行 DirectAccess 設定，您可以使用 Windows PowerShell 指令碼。  
  
##  <a name="BKMK_AddRAM"></a>步驟 1： 新增到您 server 的遠端存取管理工具  
  
#### <a name="to-add-remote-access-management-tools"></a>若要新增遠端存取管理工具  
  
1.  在伺服器上，在左下角的 [開始] 頁面上，按一下 [**伺服器管理員**圖示。  
  
     在 Windows Server Essentials，您將需要搜尋伺服器管理員打開它。 在 [開始] 頁面上，輸入**伺服器管理員**，然後按一下 [**伺服器管理員**中的搜尋結果。 若要將伺服器管理員釘選到 [開始] 畫面、 上伺服器管理員中搜尋結果中，按一下滑鼠右鍵，然後按一下**釘選到開始畫面]**。  
  
2.  如果**使用者 Account 控制項**會顯示警告訊息，按**是**。  
  
3.  在伺服器管理員儀表板中，按一下**管理**，然後按一下 [**新增角色與功能**。  
  
4.  新增角色與功能精靈中，執行下列動作：  
  
    1.  在**安裝類型**頁面上，按**以角色為基礎，或為基礎的功能的安裝**。  
  
    2.  在**伺服器選取項目頁面**(或**選取目的伺服器**Windows Server Essentials] 頁面)，按一下 [**伺服器集區中選取 [伺服器**。  
  
    3.  在**功能**頁面中，展開**遠端伺服器管理工具 （已安裝）**，展開 [**遠端存取管理工具 （已安裝）**，展開 [**角色管理工具 （已安裝）**，展開 [**遠端存取管理工具**，，然後選取 [**遠端存取 GUI 及命令列工具**。  
  
    4.  請依照下列指示完成精靈。  
  
##  <a name="BKMK_AddStaticIP"></a>步驟 2： 靜態 IP 位址變更網路介面卡伺服器的位址  
 DirectAccess 必須靜態 IP 位址的介面卡。 您需要變更您的伺服器上的區域網路介面卡的 IP 位址。  
  
#### <a name="to-add-a-static-ip-address"></a>若要新增的靜態 IP 位址  
  
1.  在 [開始] 頁面上，請打開**[控制台]**。  
  
2.  按一下**網路和網際網路**，然後按**網路狀態和工作檢視**。  
  
3.  在工作窗格中的**網路和共用中心]**，按一下 [**變更介面卡設定**。  
  
4.  本機網路介面卡，以滑鼠右鍵按一下，然後按一下**屬性**。  
  
5.  在**網路**索引標籤上，按一下 [**網際網路通訊協定第 4 版本 (TCP 日 IPv4)**，然後按一下 [**屬性**。  
  
6.  在**一般**索引標籤上，按**使用下列的 IP 位址**，然後輸入您想要使用的 IP 位址。  
  
     子網路遮罩的預設值出現自動在**子網路遮罩**方塊。 接受預設值，或輸入您想要使用的子網路遮罩值。  
  
7.  在**預設閘道**方塊中，輸入您的預設閘道 IP 位址。  
  
8.  在**慣用 DNS 伺服器]**方塊中，輸入您的 DNS 伺服器的 IP 位址。  
  
    > [!NOTE]
    >  使用的 IP 位址指派給您的網路介面卡 DHCP (例如，192.168.X.X)，而不是網路回送 (例如，127.0.0.1)。 若要指定 IP 位址，請執行**ipconfig**在 [命令提示字元。  
  
9. 在**其他 DNS 伺服器**方塊中，輸入您替代的 DNS 伺服器的 IP 位址，如果有任何。  
  
10. 按一下**[確定]**，然後按**關閉**。  
  
> [!IMPORTANT]
>  請確定您設定路由器向前連接埠 80 和 443 伺服器的新靜態 IP 位址。  
  
##  <a name="BKMK_DNS"></a>步驟 3： 準備網路位置伺服器的憑證和 DNS 記錄  
 若要準備網路位置伺服器的憑證和 DNS 記錄，執行下列工作：  
  
-   [步驟 3a： 完整存取權限授 Authenticated Users 與 Web 伺服器的憑證範本](#BKMK_GrantFullPermissions)  
  
-   [步驟 3b： 註冊一般的名稱解析外部網路，網路位置伺服器的憑證](#BKMK_EnrollaCertificate)  
  
-   [步驟 3 c： 新增新的主機上的 DNS 伺服器，並將它對應至的 Windows Server Essentials 伺服器的位址。](#BKMK_MapNewHosttoServerAddress)  
  
###  <a name="BKMK_GrantFullPermissions"></a>步驟 3a： 完整存取權限授 Authenticated Users 與 Web 伺服器的憑證範本  
 驗證使用者的憑證授權單位網頁伺服器的憑證範本完整存取權限授與是您第一項工作。  
  
####  <a name="BKMK_ToGrantFullPermissions"></a>Web 伺服器的驗證使用者完整權限授與 s 憑證範本  
  
1.  在**[開始]**頁面上，開放**憑證授權單位**。  
  
2.  主控台中在**憑證授權單位 （本機）**，展開 [ **< servername\ >-CA**，以滑鼠右鍵按一下**憑證範本**，，然後按一下**管理**。  
  
3.  在**憑證授權單位 （本機）**，以滑鼠右鍵按一下**網頁伺服器**，然後按一下 [**屬性**。  
  
4.  網頁伺服器] 功能表中上**安全性**索引標籤上，按一下 [ **Authenticated Users**、 選取**完全控制**，，然後按一下**[確定]**。  
  
5.  重新開機**Active Directory 憑證服務**。 在 [控制台] 中，請打開**檢視 [本機服務**。 在清單中的服務，以滑鼠右鍵按一下**Active Directory 憑證服務**，然後按**重新開機**。  
  
###  <a name="BKMK_EnrollaCertificate"></a>步驟 3b： 註冊一般的名稱解析外部網路，網路位置伺服器的憑證  
 接下來，註冊一般的名稱解析外部網路，網路位置伺服器的憑證。  
  
####  <a name="BKMK_ToEnrollaCertificate"></a>若要註冊網路位置伺服器的憑證  
  
1.  在**[開始]**頁面上，開放**mmc** (Microsoft Management Console)。  
  
2.  如果**使用者 Account 控制項**會顯示警告訊息，按**是**。  
  
     Microsoft Management Console (MMC) 開啟。  
  
3.  在**檔案**功能表上，按**新增/移除嵌入式管理單元**。  
  
4.  在**新增或遠端嵌入式管理單元]**方塊中，按一下 [**憑證**，然後按一下 [**新增**。  
  
5.  在**憑證嵌入式管理單元**頁面上，按一下 [**電腦 account**，然後按一下 [**下一步**。  
  
6.  在**選擇電腦**頁面上，按一下 [**本機電腦**，按一下 [**完成**，，然後按一下**[確定]**。  
  
7.  在主控台中，展開**憑證 （本機電腦）**，展開**個人**，以滑鼠右鍵按一下**憑證**，然後在**所有工作**，按一下**要求新的憑證**。  
  
8.  憑證註冊精靈出現時，按一下**下一步**。  
  
9. 在**選擇憑證註冊原則**頁面上，按一下 [**下**。  
  
10. 在**要求憑證**頁面上，選取**網頁伺服器**核取方塊，，然後按一下 [**所需的詳細資訊註冊這個憑證**。  
  
11. 在**憑證屬性**方塊中，輸入下列設定為**主體名稱**:  
  
    1.  適用於**輸入**、**一般名稱**。  
  
    2.  適用於**值**，輸入名稱的網路位置伺服器 (例如，DirectAccess-NLS.contoso.local)，然後按一下 [**新增]**。  
  
    3.  按一下**[確定]**，然後按**Enroll**。  
  
12. 當憑證註冊完成時，按**完成**。  
  
###  <a name="BKMK_MapNewHosttoServerAddress"></a>步驟 3 c： 新增新的主機上的 DNS 伺服器，並將它對應的 Windows Server Essentials 伺服器地址  
 若要完成 DNS 設定，新增新的主機上的 DNS 伺服器，並將它對應至的 Windows Server Essentials 伺服器的位址。  
  
####  <a name="BKMK_ToMapNewHosttoServerAddress"></a>若要對應主機新的 Windows Server Essentials 伺服器地址  
  
1.  在 [開始] 頁面上，開放 DNS 管理員。 若要打開 DNS 管理員，搜尋**dnsmgmt.msc**，然後按一下 [ **dnsmgmt.msc**在結果中。  
  
2.  DNS Manager 主控台樹上，依序展開本機伺服器、**正向對應區域**，以滑鼠右鍵按一下伺服器的網域尾碼的區域，然後按一下**新主機 （或 AAAA）**。  
  
3.  輸入名稱與 (例如，NLS.contoso.local DirectAccess) 伺服器的 IP 位址，以及其對應伺服器位址 (例如，192.168.x.x)。  
  
4.  按一下**新增主機**，按一下 [ **[確定]**，然後按一下 [**完成**。  
  
##  <a name="BKMK_AddSecurityGroup"></a>步驟 4： 建立 DirectAccess client 電腦安全性群組  
 接下來，建立使用 DirectAccess client 電腦安全性群組，然後將電腦帳號新增到群組。  
  
#### <a name="to-add-a-security-group-for-client-computers-that-use-directaccess"></a>若要新增用 DirectAccess client 電腦安全性群組  
  
1.  在伺服器管理員儀表板中，按一下**工具**，然後按一下 [ **Active Directory 使用者和電腦**。  
  
    > [!NOTE]
    >  如果您看不到**Active Directory 使用者和電腦**在**工具**功能表，您需要安裝的功能。 若要安裝 Active Directory 使用者和群組，系統管理員身分執行下列 Windows PowerShell cmdlet: `Install-WindowsFeature RSAT-ADDS-Tools`。 如需詳細資訊，請查看[安裝或移除遠端伺服器管理工具套件](https://technet.microsoft.com/library/cc730825.aspx)。  
  
2.  主控台中，展開伺服器、 以滑鼠右鍵按一下**使用者**，按一下 [**新**，然後按一下 [**群組**。  
  
3.  輸入群組的名稱、 群組範圍，以及群組類型 （建立安全性群組），然後按一下**[確定]**。  
  
 新的安全性群組新增至**使用者**資料夾。  
  
#### <a name="to-add-computer-accounts-to-the-security-group"></a>若要將電腦新增帳號安全性群組  
  
1.  在伺服器管理員儀表板中，按一下**工具**，然後按一下 [ **Active Directory 使用者和電腦**。  
  
2.  主控台中展開 [伺服器]，然後按一下**使用者**。  
  
3.  在帳號及伺服器上的安全性群組的清單中，以滑鼠右鍵按一下您建立的 DirectAccess，安全性群組，然後按一下**屬性**。  
  
4.  在**成員**索引標籤上，按**新增]**。  
  
5.  在對話方塊中，輸入您想要加入該群組，並名稱以 （;） 分號來分隔電腦帳號的名稱。 然後按一下**檢查名稱]**。  
  
6.  電腦帳號驗證之後，請按一下**[確定]**。 然後按一下**[確定]**再試一次。  
  
> [!NOTE]
>  您也可以使用**的成員**索引標籤中新增 account 安全性群組的電腦 account 屬性。  
  
##  <a name="BKMK_EnableConfigureDA"></a>步驟 5： 讓和設定 DirectAccess  
 讓和 Windows Server Essentials 中設定 DirectAccess，您必須完成以下步驟：  
  
-   [步驟 5a: DirectAccess 可以使用遠端存取管理主控台](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
-   [步驟 5b： 移除無效 IPv6Prefix RRAS GPO (僅限 Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
-   [步驟 5 c： 讓 client 電腦執行的 Windows 7 企業版使用 DirectAccess](#BKMK_Step4cWindows7Setup)  
  
-   [步驟 5 d︰ 將網路位置伺服器設定](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
-   [步驟 5e： 新增略過 CA 憑證，當您建立 IPsec 頻道登錄按鍵](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
###  <a name="BKMK_EnableDA"></a>步驟 5a: DirectAccess 可以使用遠端存取管理主控台  
 本章節提供支援在 Windows Server Essentials DirectAccess 逐步指示。 如果您有 VPN 伺服器上您尚未設定，您應該此程序您在開始之前，做。 適用於的指示，請查看[管理 VPN](Manage-VPN-in-Windows-Server-Essentials.md)。  
  
##### <a name="to-enable-directaccess-by-using-the-remote-access-management-console"></a>若要讓 DirectAccess 使用遠端存取管理主控台  
  
1.  在 [開始] 頁面上，請打開**遠端存取管理**。  
  
2.  在讓 DirectAccess 精靈中，執行下列動作：  
  
    1.  檢視**DirectAccess 必要條件**，按一下 [**下**。  
  
    2.  在**選擇群組**索引標籤，新增您先前建立的 DirectAccess 戶端安全性群組。 (如果您無法建立安全性群組，請查看[執行 「 步驟 4： 建立電腦安全性群組 DirectAccess client 的](#BKMK_AddSecurityGroup)的指示操作。)  
  
    3.  在**選取的群組**索引標籤上，按一下 [**讓 DirectAccess 僅限行動裝置版電腦的**如果您想要讓行動裝置版的電腦使用 DirectAccess 遠端存取伺服器，並再按**下一步**。  
  
    4.  在**網路拓撲**，選取的伺服器，拓撲，然後按**下**。  
  
    5.  在**DNS 尾碼搜尋清單**，視需要新增其他 DNS 尾碼 client 電腦，然後按**下**。  
  
        > [!NOTE]
        >  根據預設，讓 DirectAccess 精靈已經加入網域目前 DNS 尾碼。 不過，您可以新增多個視。  
  
    6.  檢視群組原則物件 (Gpo) 會套用到，並視需要進行修改。  
  
    7.  按一下**下一步**，然後按**完成**。  
  
    8.  重新存取的遠端管理服務來提升權限模式中執行下列 Windows PowerShell 命令：  
  
        ```powershell  
        Restart-Service RaMgmtSvc   
        ```  
  
###  <a name="BKMK_RemoveIPv6"></a>步驟 5b： 移除無效 IPv6Prefix RRAS GPO (僅限 Windows Server Essentials)  
  本節適用於執行 Windows Server Essentials 的伺服器。  
  
 打開 Windows PowerShell 系統管理員的身分，執行下列命令：  
  
```powershell  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
```  
  
###  <a name="BKMK_Step4cWindows7Setup"></a>步驟 5 c： 讓 client 電腦執行的 Windows 7 企業版使用 DirectAccess  
 如果您有 client 電腦執行的 Windows 7 企業版，完成下列步驟，讓 DirectAccess 從那些電腦。  
  
##### <a name="to-enable--windows-7-enterprise-computers-to-use-directaccess"></a>若要讓 Windows 7 企業版電腦進行使用 DirectAccess  
  
1.  在伺服器 s 的 [開始] 畫面，請打開**遠端存取管理**。  
  
2.  在遠端存取管理主控台中，按一下 [**設定**。 然後，在**設定的詳細資料**窗格中，在**執行 「 步驟 2**，按一下 [**編輯**。  
  
     遠端存取伺服器安裝精靈開啟。  
  
3.  在**驗證**索引標籤上，選擇將 （您可以選擇 Windows Server Essentials 伺服器的憑證） 的受信任的根憑證憑證授權單位憑證。 按一下**讓 Windows 7 client 電腦連接透過 DirectAccess**，然後按一下 [**下一步**。  
  
4.  請依照下列指示完成精靈。  
  
> [!IMPORTANT]
>  還有一個已知的問題，適用於 Windows 7 企業版電腦連接到 DirectAccess，如果 Windows Server Essentials 伺服器未隨附 UR1 預先安裝。 若要讓 DirectAccess 連接的環境中，您必須執行其他步驟執行：  
>   
>  1.  Hotfix 中所述[Microsoft 知識庫 (KB) 文件 2796394](https://support.microsoft.com/kb/2796394) Windows Server Essentials 伺服器上。 然後，重新伺服器。  
> 2.  然後 hotfix 中所述[Microsoft 知識庫 (KB) 文件 2615847](https://support.microsoft.com/kb/2615847)在每個 Windows 7 的電腦上。  
>   
>      此問題已在 Windows Server Essentials 解析。  
  
###  <a name="BKMK_NLS"></a>步驟 5 d︰ 將網路位置伺服器設定  
 本章節提供逐步指示執行設定的網路位置伺服器設定。  
  
> [!NOTE]
>  在您開始之前，請複製到 < SystemDrive\ > \inetpub\wwwroot 資料夾 < SystemDrive\ > \Program Files\Windows Server\Bin\WebApps\Site\insideoutside 資料夾。 也複製 < SystemDrive\ > \Program Files\Windows Server\Bin\WebApps\Site 資料夾 < SystemDrive\ > \Program Files\Windows Server\Bin\WebApps\Site\insideoutside 資料夾 default.aspx 檔案。  
  
##### <a name="to-configure-the-network-location-server"></a>若要設定的網路位置伺服器  
  
1.  在 [開始] 頁面上，請打開**遠端存取管理**。  
  
2.  在遠端存取管理主控台中，按一下 [**設定**，並在**遠端存取安裝**中的詳細資料窗格中，**執行 「 步驟 3**，按一下**編輯**。  
  
3.  在遠端存取伺服器安裝精靈中，在**網路位置伺服器**索引標籤，選取**部署遠端存取伺服器上的網路位置伺服器**，，然後選取 [先前發行的憑證 (中[執行 「 步驟 3： 準備網路位置伺服器的憑證和 DNS 記錄](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS))。  
  
4.  請依照下列指示完成精靈，並再按**完成**。  
  
###  <a name="BKMK_CA"></a>步驟 5e： 新增略過 CA 憑證，當您建立 IPsec 頻道登錄按鍵  
 您的下一個步驟是設定來建立 IPsec 頻道時，請略過 CA 憑證伺服器。  
  
##### <a name="to-add-a-registry-key-to-bypass-the-ca-certification"></a>若要新增略過 CA 憑證登錄按鍵  
  
1.  在 [開始] 頁面上，請打開**regedit** （作業系統）。  
  
2.  在作業系統，展開**跳**，展開**系統**，展開 [ **CurrentControlSet**，展開 [**服務**，展開 [ **IKEEXT**。  
  
3.  在**IKEEXT**，以滑鼠右鍵按一下**參數**，按一下 [**新**，然後按一下 [ **DWORD （32 位元） 值**。  
  
4.  重新命名新加入的值，以**ikeflags**。  
  
5.  按兩下**ikeflags**設定，**輸入**以**十六進位**，將值設定為**8000**，，然後按一下 [ **[確定]**。  
  
> [!NOTE]
>  您可以使用下列 Windows PowerShell 命令提升權限模式，新增登錄該鍵：  
>   
>  `Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000`  
  
##  <a name="BKMK_NRPT"></a>步驟 6： 設定 DirectAccess 伺服器的名稱解析原則的資料表設定  
 本節指示 DirectAccess client gpo，編輯內部位址 （例如項目以 contoso.local 尾碼） 的名稱解析原則的資料表 (NPRT) 項目，然後設定 IPHTTPS 介面地址。  
  
#### <a name="to-configure-name-resolution-policy-table-entries"></a>若要設定的名稱解析原則的資料表項目  
  
1.  在 [開始] 頁面上，請打開**群組原則管理**。  
  
2.  在群組原則管理主控台中，按一下 [預設樹系和網域，以滑鼠右鍵按一下**DirectAccess Client 設定**，然後按**編輯**。  
  
3.  按一下**電腦設定**，按一下 [**原則**，按一下 [**的 Windows 設定**，，然後按一下**名稱解析原則**。 選擇命名空間與您的 DNS 尾碼相同的項目，然後按一下**編輯規則**。  
  
4.  按一下**適用於 DirectAccess DNS 設定**索引標籤。然後選取 [**在此規則 DirectAccess 讓 DNS 設定**。 在 [DNS 伺服器清單新增 IP-HTTPS 介面 IPv6 位址。  
  
    > [!NOTE]
    >  您可以使用下列 Windows PowerShell 命令來取得 IPv6 位址：  
    >   
    >  `(Get-NetIPInterface -InterfaceAlias IPHTTPSInterface | Get-NetIPAddress -PrefixLength 128)[1].IPAddress`  
  
##  <a name="BKMK_TCPUDP"></a>步驟 7： 設定 DirectAccess 伺服器 Gpo 的 TCP 與 UDP 免  
 本節設定 DirectAccess 伺服器 Gpo 的 TCP 與 UDP 免逐步指示。  
  
#### <a name="to-configure-firewall-rules"></a>若要設定免  
  
1.  在 [開始] 頁面上，請打開**群組原則管理**。  
  
2.  在群組原則管理主控台中，按一下 [預設樹系和網域，以滑鼠右鍵按一下**DirectAccess 伺服器設定**，然後按**編輯**。  
  
3.  按一下**電腦設定**，按一下 [**原則**，按一下 [ **Windows 設定**，按一下**的安全性設定**，按一下 [ **Windows 防火牆使用進階安全性**，按一下 [下一步層級**Windows 防火牆使用進階安全性**，，然後按一下 [**輸入規則**。 以滑鼠右鍵按一下**網域名稱 （TCP 單元） 伺服器**，然後按**屬性**。  
  
4.  按一下**範圍**索引標籤，在**區域 IP 位址**清單中，將新增的 IP-HTTPS 介面 IPv6 位址。  
  
5.  重複執行相同的程序適用於**（UDP 入） 的網域名稱伺服器**。  
  
##  <a name="BKMK_DNS64"></a>若要聆聽 IP-HTTPS 介面執行 「 步驟 8: DNS64 變更設定  
 您必須變更聆聽使用 Windows PowerShell 命令下列的 IP-HTTPS 介面 DNS64 設定。  
  
```powershell  
Set-NetDnsTransitionConfiguration  �AcceptInterface IPHTTPSInterface  
```  
  
##  <a name="BKMK_ExemptPort"></a>步驟 9： 保留 WinNat 服務連接埠  
 使用下列的 Windows PowerShell 命令保留 WinNat 服務連接埠。 取代您的 Windows Server Essentials 伺服器的實際 IPv4 位址] 192.168.1.100 」。  
  
```powershell  
Set-NetNatTransitionConfiguration  �IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
```  
  
> [!IMPORTANT]
>  若要避免連接埠衝突應用程式，請務必您保留 WinNat 服務連接埠範圍不包含 6602 連接埠。  
  
##  <a name="BKMK_WinNAT"></a>步驟 10： 重新開機 WinNat 服務  
 將 Windows NAT 驅動程式 (WinNat) 服務來執行下列 Windows PowerShell 命令重新開機。  
  
```powershell  
Restart-Service winnat  
```  
  
##  <a name="BKMK_AppendixBPowerShellScript"></a>使用 Windows PowerShell 來 DirectAccess 設定附錄：  
 本章節告訴您如何設定及使用 Windows PowerShell 來設定 DirectAccess。  
  
### <a name="preparation"></a>準備  
 設定您的伺服器的 DirectAccess 您開始之前，您必須先完成下列動作：  
  
1.  依照中的程序[執行 「 步驟 3： 準備網路位置伺服器的憑證和 DNS 記錄](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)新使用者註冊憑證名為**DirectAccess-NLS.contoso.com** (位置**contoso.com**由您的實際內部網域名稱取代)，以及新增 DNS 記錄網路位置伺服器 (NLS)。  
  
2.  新增安全性群組名為**DirectAccessClients**在 Active Directory，再新增 client 電腦，您想要提供 DirectAccess 的功能。 如需詳細資訊，請查看[執行 「 步驟 4： 建立電腦安全性群組 DirectAccess client 的](#BKMK_AddSecurityGroup)。  
  
### <a name="commands"></a>命令  
  
> [!IMPORTANT]
>  Windows Server Essentials，您不需要不必要 IPv6 前置詞 GPO 中移除。 Delete 的程式碼區段，以下列標籤： `# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO`。  
  
```powershell  
# Add Remote Access role if not installed yet  
$ra = Get-WindowsFeature RemoteAccess  
If ($ra.Installed -eq $FALSE) { Add-WindowsFeature RemoteAccess }  
  
# Server may need to restart if you installed RemoteAccess role in the above step  
  
# Set the internet domain name to access server, replace contoso.com below with your own domain name  
$InternetDomain = "www.contoso.com"  
#Set the SG name which you create for DA clients  
$DaSecurityGroup = "DirectAccessClients"  
#Set the internal domain name  
$InternalDomain = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().Name  
  
# Set static IP and DNS settings  
$NetConfig = Get-WmiObject Win32_NetworkAdapterConfiguration -Filter "IPEnabled=$true"  
$CurrentIP = $NetConfig.IPAddress[0]  
$SubnetMask = $NetConfig.IPSubnet | Where-Object{$_ -like "*.*.*.*"}  
$NetConfig.EnableStatic($CurrentIP, $SubnetMask)  
$NetConfig.SetGateways($NetConfig.DefaultIPGateway)  
$NetConfig.SetDNSServerSearchOrder($CurrentIP)  
  
# Get physical adapter name and the certificate for NLS server  
$Adapter = (Get-WmiObject -Class Win32_NetworkAdapter -Filter "NetEnabled=$true").NetConnectionId  
$Certs = dir cert:\LocalMachine\My  
$nlscert = $certs | Where-Object{$_.Subject -like "*CN=DirectAccess-NLS*"}  
  
# Add regkey to bypass CA cert for IPsec authentication  
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000  
  
# Install DirectAccess.   
Install-RemoteAccess -NoPrerequisite -DAInstallType FullInstall -InternetInterface $Adapter -InternalInterface $Adapter -ConnectToAddress $InternetDomain -nlscertificate $nlscert -force  
  
#Restart Remote Access Management service  
Restart-Service RaMgmtSvc  
  
# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO   
  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
  
# Enable client computers running Windows 7 to use DirectAccess  
$allcertsinroot = dir cert:\LocalMachine\root  
$rootcert = $allcertsinroot | Where-Object{$_.Subject -like "*-CAA*"}  
Set-DAServer  �IPSecRootCertificate $rootcert[0]  
Set  �DAClient  �OnlyRemoteComputers Disabled -Downlevel Enabled  
  
#Set the appropriate security group used for DA client computers. Replace the group name below with the one you created for DA clients  
Add-DAClient -SecurityGroupNameList $DaSecurityGroup   
Remove-DAClient -SecurityGroupNameList "Domain Computers"  
  
# Gather DNS64 IP address information  
$Remoteaccess = get-remoteaccess  
$IPinterface = get-netipinterface -InterfaceAlias IPHTTPSInterface | get-netipaddress -PrefixLength 128  
$DNS64IP=$IPInterface[1].IPaddress  
$Natconfig = Get-NetNatTransitionConfiguration  
  
# Configure TCP and UDP firewall rules for the DirectAccess server GPO  
$GpoName = 'GPO:'+$InternalDomain+'\DirectAccess Server Settings'  
Get-NetFirewallRule -PolicyStore $GpoName -Displayname "Domain Name Server (TCP-IN)"|Get-NetFirewallAddressFilter | Set-NetFirewallAddressFilter -LocalAddress $DNS64IP  
Get-NetFirewallrule -PolicyStore $GpoName -Displayname "Domain Name Server (UDP-IN)"|Get-NetFirewallAddressFilter | Set-NetFirewallAddressFilter -LocalAddress $DNS64IP  
  
# Configure the name resolution policy settings for the DirectAccess server, replace the DNS suffix below with the one in your domain  
$Suffix = '.' + $InternalDomain  
set-daclientdnsconfiguration -DNSsuffix $Suffix -DNSIPAddress $DNS64IP  
  
# Change the DNS64 configuration to listen to IP-HTTPS interface  
Set-NetDnsTransitionConfiguration -AcceptInterface IPHTTPSInterface  
  
# Copy the necessary files to NLS site folder  
XCOPY 'C:\inetpub\wwwroot' 'C:\Program Files\Windows Server\Bin\WebApps\Site\insideoutside' /E /Y  
XCOPY 'C:\Program Files\Windows Server\Bin\WebApps\Site\Default.aspx' 'C:\Program Files\Windows Server\Bin\WebApps\Site\insideoutside' /Y  
  
# Reserve ports for the WinNat service  
Set-NetNatTransitionConfiguration  �IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
  
# Restart the WinNat service  
Restart-Service winnat  
```  
  
## <a name="see-also"></a>也了  
  
-   [管理隨時隨地存取](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
