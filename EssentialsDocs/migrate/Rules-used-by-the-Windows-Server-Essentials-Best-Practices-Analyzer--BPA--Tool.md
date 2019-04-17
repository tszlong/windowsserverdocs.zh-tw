---
title: "Windows Server Essentials 最佳做法分析 (BPA) 工具的使用規則"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37e1dae7-586c-4dd7-bf83-7e14a9567c8f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c205bc8ff75bf64d4a13a7d799988c9d1ebe1a22
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="rules-used-by-the-windows-server-essentials-best-practices-analyzer-bpa-tool"></a>Windows Server Essentials 最佳做法分析 (BPA) 工具的使用規則

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

此文章將描述用來 Windows Server Essentials 最佳做法分析 (BPA) 的規則。 BPA 會檢查您執行的 Windows Server Essentials 並提供的報告描述問題，並提供建議解析他們的伺服器。 建議的 product 支援 Windows Server Essentials 的組織所開發。  
  
## <a name="using-the-tool"></a>使用此工具  
 是的標準做法，當您移轉到 Windows Server Essentials 從 Windows Server 2011 Essentials、Windows 小型企業 Server 2011 Essentials 或 Windows Home Server 2011、BPA 執行目的伺服器上，當您完成您的設定和資料移轉。 您可以隨時從儀表板執行此工具。  
  
#### <a name="to-run-the--windows-server-essentials-bpa-on-the-server"></a>若要在伺服器上執行 Windows Server Essentials BPA  
  
1.  伺服器以系統管理員的身分登入，然後打開儀表板。  
  
2.  儀表板，請按一下**裝置**索引標籤。  
  
3.  在**伺服器工作**窗格中，按**最佳做法分析**。  
  
4.  檢視每個 BPA 訊息，並依照指示修正必要的相關問題。  
  
## <a name="rules-used-by-the-best-practices-analyzer"></a>使用的最佳做法分析規則  
  
### <a name="disable-ip-filtering"></a>停用 IP 篩選  
 **問題：**在伺服器上目前支援篩選的 IP。 您必須停用 IP 篩選。  
  
 **影響：**如果 IP 篩選功能，可能會封鎖網路流量。  
  
 **解析度：**  
  
##### <a name="to-disable-ip-filtering"></a>若要停用 IP 篩選  
  
1.  打開 regedit.exe 伺服器上。  
  
2.  瀏覽至 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters。  
  
3.  以滑鼠右鍵按一下**EnableSecurityFilters**，然後按**修改]**。  
  
4.  在**編輯 DWORD（32 位元）值**視窗中，變更**數值資料**欄位零，以，然後按一下 [ **[確定]**。  
  
5.  若要套用的變更，重新開機伺服器。  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-set-to-start-automatically-by-default"></a>散發交易協調者 (MSDTC) 服務應設為預設會自動開始  
 **問題：**未 MSDTC 服務會自動開始設定  
  
 **影響：** MSDTC 服務，可能會不開始自動伺服器時開始。 如果已停止服務，某些 SQL Server 或 COM 功能可能會失敗。 如此一來，使用 Microsoft SQL Server 或 COM 功能的應用程式可能無法正常運作。  
  
 **解析度：**  
  
##### <a name="to-configure-the-msdtc-service-to-start-automatically"></a>若要設定 MSDTC 服務會自動開始  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**散發交易協調器**服務，並再按**屬性**。  
  
3.  在**一般**索引標籤上，變更**開機輸入**以**自動（延遲開始）**，，然後按一下**[確定]**。  
  
### <a name="the-netlogon-service-should-be-configured-to-start-automatically-by-default"></a>應該會自動開始預設設定 Netlogon 服務  
 **問題：**未設定 Netlogon 服務會自動開始。  
  
 **影響：** Netlogon 服務，可能會不開始自動伺服器時開始。 如果已停止服務，伺服器可能驗證使用者與服務。  
  
 **解析度：**  
  
##### <a name="to-configure-the-netlogon-service-to-start-automatically"></a>若要設定 Netlogon 服務會自動開始  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**Netlogon**服務，並再按**屬性**。  
  
3.  在**一般**索引標籤上，變更**開機類型**以**自動**，然後按一下 [ **[確定]**。  
  
### <a name="the-dns-client-service-should-be-configured-to-start-automatically-by-default"></a>DNS Client 服務應會設定為自動預設的 [開始]  
 **問題：**未設定 DNS Client 服務會自動開始。  
  
 **影響：**的 DNS Client 服務，可能會不開始自動伺服器時開始。 如果已停止這項服務，伺服器可能無法解析 DNS 名稱。  
  
 **解析度：**  
  
##### <a name="to-configure-the-dns-client-service-to-start-automatically"></a>若要設定 DNS Client 服務會自動開始  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**DNS Client**服務，並再按**屬性**。  
  
3.  在**一般**索引標籤上，變更**開機類型**以**自動**，然後按一下 [ **[確定]**。  
  
### <a name="the-dns-server-service-should-be-configured-to-start-automatically-by-default"></a>DNS 伺服器服務應會設定為自動預設的 [開始]  
 **問題：**未設定 DNS 伺服器服務會自動開始。  
  
 **影響：**的 DNS 伺服器的服務，可能會不開始自動伺服器時開始。 如果這項服務已停止，將不會 DNS 更新。  
  
 **解析度：**  
  
##### <a name="to-configure-the-dns-server-service-to-start-automatically"></a>若要設定 DNS 伺服器服務會自動開始  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**的 DNS 伺服器**服務，並再按**屬性**。  
  
3.  在**一般**索引標籤上，變更**開機類型**以**自動**，然後按一下 [ **[確定]**。  
  
### <a name="active-directory-web-services-is-not-set-to-the-default-start-mode"></a>Active Directory 網頁的服務不是設定為預設的 [開始] 畫面模式  
 **問題：** Active Directory 網頁的服務不設定為預設的 [開始] 畫面模式的自動。  
  
 **影響：**未設定成預設的 [開始] 畫面模式的自動 Active Directory Web 服務 (ADWS)。 如果在伺服器上的 ADWS 停止或停用，client 應用程式的 Windows PowerShell 模組 Active Directory 或 Active Directory 管理中心無法管理或存取此伺服器上執行 directory 服務執行個體。 如需詳細資訊，請查看[AD DS 中的新功能：Active Directory Web 服務](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx)(https://technet.microsoft.com/library/dd391908(WS.10).aspx) 在 Windows Server Technical 媒體櫃。  
  
 **解析度：**  
  
##### <a name="to-configure-the-active-directory-web-services-service-to-start-automatically"></a>若要設定 Active Directory Web 服務服務會自動開始  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**Active Directory Web 服務**服務，並再按**屬性**。  
  
3.  在**一般**索引標籤上，變更**開機類型**以**自動**，然後按一下 [ **[確定]**。  
  
### <a name="the-dhcp-client-service-should-be-configured-to-start-automatically-by-default"></a>應該會自動開始預設設定 DHCP Client 服務  
 **問題：**未設定 DHCP Client 服務會自動開始。  
  
 **影響：** DHCP Client 服務不會開始自動伺服器時開始。 如果已停止這項服務，client 電腦無法收到伺服器的 IP 位址。  
  
 **解析度：**  
  
##### <a name="to-configure-the-dhcp-client-service-to-start-automatically"></a>若要設定 DHCP Client 服務會自動開始  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**DHCP Client**服務，並再按**屬性**。  
  
3.  在**一般**索引標籤上，變更**開機類型**以**自動**，然後按一下 [ **[確定]**。  
  
### <a name="the-iis-admin-service-should-be-configured-to-start-automatically-by-default"></a>IIS 管理服務應會設定為預設會自動開始  
 **問題：**未設定 IIS 管理服務會自動開始。  
  
 **影響：** IIS 管理員服務不會開始自動伺服器時開始。 如果這項服務已停止，您可能無法存取伺服器，例如存取網路上執行的網站。  
  
 **解析度：**  
  
##### <a name="to-configure-the-iis-admin-service-to-start-automatically"></a>若要設定 IIS 管理服務會自動開始  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**IIS 管理服務**，然後按一下 [**屬性**。  
  
3.  在**一般**索引標籤上，變更**開機類型**以**自動**，然後按一下 [ **[確定]**。  
  
### <a name="the-world-wide-web-publishing-service-should-be-configured-to-start-automatically-by-default"></a>應該設定預設會自動開始在全球發行服務  
 **問題：**未設定自動開始在全球發行服務。  
  
 **影響：**在全球發行服務，可能會不開始自動伺服器時開始。 如果這項服務已停止，您可能無法存取伺服器，例如存取網路上執行的網站。  
  
 **解析度：**  
  
##### <a name="to-configure-the-world-wide-web-publishing-service-to-start-automatically"></a>若要設定的全球發行服務會自動開始  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**全球發行服務**，然後按**屬性**。  
  
3.  在**一般**索引標籤上，變更**開機類型**以**自動**，然後按一下 [ **[確定]**。  
  
### <a name="the-remote-registry-service-should-be-configured-to-start-automatically-by-default"></a>遠端登錄服務應會設定為自動預設的 [開始]  
 **問題：**未設定遠端登錄服務會自動開始。  
  
 **影響：**  
  
 伺服器開始時遠端登錄服務可能會不會自動開始。 如果這項服務已停止，您可能無法執行某些網路作業遠端。  
  
 **解析度：**  
  
##### <a name="to-configure-the-remote-registry-service-to-start-automatically"></a>設定遠端登錄服務會自動開始  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**遠端登錄**服務，並再按**屬性**。  
  
3.  在**一般**索引標籤上，變更**開機類型**以**自動**，然後按一下 [ **[確定]**。  
  
### <a name="the-remote-desktop-gateway-service-should-be-configured-to-start-automatically-by-default"></a>遠端桌面閘道服務應會設定為自動預設的 [開始]  
 **問題：**未設定遠端桌面閘道服務會自動開始。  
  
 **影響：**如果停止這項服務，可能無法存取使用遠端 Web 存取電腦的使用者。  
  
 **解析度：**  
  
##### <a name="to-configure-the-remote-desktop-gateway-service-to-start-automatically"></a>設定遠端桌面閘道服務會自動開始  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**遠端桌面閘道**服務，並再按**屬性**。  
  
3.  在**一般**索引標籤上，變更**開機輸入**以**自動（延遲開始）**，，然後按一下**[確定]**。  
  
### <a name="the-windows-time-service-should-be-configured-to-start-automatically-by-default"></a>Windows 時間服務應會設定為預設會自動開始  
 **問題：** Windows 時間服務會自動開始未設定。  
  
 **影響：**如果停止這項服務，資料與的時間同步則無法使用。  
  
 **解析度：**  
  
##### <a name="to-configure-the-windows-time-service-to-start-automatically"></a>設定 Windows 時間服務會自動開始  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**Windows 時間**服務，並再按**屬性**。  
  
3.  在**一般**索引標籤上，變更**開機類型**以**自動**，然後按一下 [ **[確定]**。  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-started"></a>應該會開始散發交易協調者 (MSDTC) 服務  
 **問題：**在伺服器上 MSDTC 服務無法執行。  
  
 **影響：**如果停止這項服務，某些 SQL Server 或 COM 功能可能會失敗。 如此一來，使用 Microsoft SQL Server 或 COM 功能的應用程式可能無法正常運作。  
  
 **解析度：**  
  
##### <a name="to-start-the-distributed-transaction-coordinator-service"></a>若要開始缺少  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**散發交易協調器**服務，並再按**[開始]**。  
  
### <a name="the-netlogon-service-should-be-started"></a>應該會開始 Netlogon 服務  
 **問題：**在伺服器上 Netlogon 服務無法執行。  
  
 **影響：**如果您不開始使用這項服務，伺服器可能驗證使用者與服務。  
  
 **解析度：**  
  
##### <a name="to-start-the-netlogon-service"></a>若要開始 Netlogon 服務  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**Netlogon**服務，並再按**[開始]**。  
  
### <a name="the-dns-client-service-should-be-started"></a>應該會開始 DNS Client 服務  
 **問題：**的 DNS Client 服務無法執行伺服器上。  
  
 **影響：**如果不開始使用這項服務，伺服器可能會無法解析 DNS 名稱。  
  
 **解析度：**  
  
##### <a name="to-start-the-dns-client-service"></a>若要開始 DNS Client 服務  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**DNS Client**服務，並再按**[開始]**。  
  
### <a name="the-dns-server-service-should-be-started"></a>應該會開始 DNS 伺服器服務  
 **問題：**的 DNS 伺服器的服務不在伺服器上執行。  
  
 **影響：**如果您不開始使用 DNS 伺服器服務，可能不會執行 DNS 更新。  
  
 **解析度：**  
  
##### <a name="to-start-the-dns-server-service"></a>若要開始 DNS 伺服器服務  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**的 DNS 伺服器**服務，並再按**[開始]**。  
  
### <a name="active-directory-web-services-is-not-started"></a>不會開始 active Directory Web 服務  
 **問題：** Active Directory 網頁的服務不開始使用。  
  
 **影響：**未開始 Active Directory Web 服務 (ADWS)。 如果在伺服器上的 ADWS 停止或停用，client 應用程式的 Windows PowerShell 模組 Active Directory 或 Active Directory 管理中心無法管理或存取此伺服器上執行 directory 服務執行個體。 如需詳細資訊，請查看[AD DS 中的新功能：Active Directory Web 服務](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx)(https://technet.microsoft.com/library/dd391908(WS.10).aspx) 在 Windows Server Technical 媒體櫃。  
  
 **解析度：**  
  
##### <a name="to-start-the-active-directory-web-services-service"></a>若要開始 Active Directory Web 服務  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**Active Directory Web 服務**，然後按**[開始]**。  
  
### <a name="the-dhcp-client-service-should-be-started"></a>應該會開始 DHCP Client 服務  
 **問題：** DHCP Client 服務無法執行伺服器上。  
  
 **影響：** client 的電腦是否會停止這項服務，無法接收來自伺服器的 IP 位址。  
  
 **解析度：**  
  
##### <a name="to-start-the-dhcp-client-service"></a>若要開始 DHCP Client 服務  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**DHCP Client**服務，並再按**[開始]**。  
  
### <a name="the-iis-admin-service-should-be-started"></a>應該會開始 IIS 管理服務  
 **問題：**系統管理員 IIS 服務不在伺服器上執行。  
  
 **影響：**如果停止這項服務，您可能無法存取伺服器，例如存取網路上執行的網站。  
  
 **解析度：**  
  
##### <a name="to-start-the-iis-admin-service"></a>若要開始 IIS 管理服務  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**IIS 管理服務**，然後按一下 [ **[開始]**。  
  
### <a name="the-world-wide-web-publishing-service-should-be-started"></a>應該會開始在全球發行服務  
 **問題：**在全球發行服務不在伺服器上執行。  
  
 **影響：**如果停止這項服務，您可能無法存取伺服器，例如存取網路上執行的網站。  
  
 **解析度：**  
  
##### <a name="to-start-the-world-wide-web-publishing-service"></a>若要開始在全球發行服務  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**全球發行服務**，然後按**[開始]**。  
  
### <a name="the-remote-desktop-gateway-service-should-be-started"></a>應該會開始遠端桌面閘道服務  
 **問題：**在伺服器上遠端桌面閘道服務無法執行。  
  
 **影響：**如果停止這項服務，可能無法存取電腦使用遠端 Web 存取的使用者。  
  
 **解析度：**  
  
##### <a name="to-start-the-remote-desktop-gateway-service"></a>若要開始遠端桌面服務閘道  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**遠端桌面閘道**服務，並再按**[開始]**。  
  
### <a name="the-windows-time-service-should-be-started"></a>Windows 時間服務應該會開始  
 **問題：** Windows 時間服務不在伺服器上執行。  
  
 **影響：**如果停止這項服務，資料與的時間同步就不會提供。  
  
 **解析度：**  
  
##### <a name="to-start-the-windows-time-service"></a>若要開始 Windows 時間服務  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**Windows 時間**服務，並再按**[開始]**。  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-logon-account-should-be-nt-authoritynetwork-service"></a>散發交易協調者 (MSDTC) 服務登入 account 應該 NT AUTHORITY\Network 服務  
 **問題：**變更散發交易協調者 (MSDTC) 服務預設登入負責。  
  
 **影響：**的服務可能不會如預期般運作所需的權限。 如此一來，使用 SQL Server 或 COM 函式應用程式可能無法正常運作。  
  
 **解析度：**  
  
##### <a name="to-change-the-logon-account-for-the-service"></a>若要變更登入 account 服務  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**散發交易協調器**服務，並再按**屬性**。  
  
3.  在**登入**索引標籤，選取**此 account**，輸入**NT AUTHORITY\Network 服務**，，然後按一下**[確定]**。  
  
### <a name="the-netlogon-service-should-use-the-local-system-account-as-its-logon-account"></a>Netlogon 服務應該使用本機系統 account 為其登入 account  
 **問題：**變更 Netlogon 服務預設登入負責。  
  
 **影響：**的服務可能不會如預期般運作所需的權限。 如此一來，伺服器可能驗證使用者與服務。  
  
 **解析度：**  
  
##### <a name="to-change-the-netlogon-service-logon-account"></a>若要變更 Netlogon 服務登入 account  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**Netlogon**服務，並再按**屬性**。  
  
3.  在**登入**索引標籤，選取**本機系統 account**。  
  
### <a name="the-dns-client-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>DNS Client 服務應做為其登入 account 使用 NT AUTHORITY\Network 服務 account  
 **問題：**變更的預設登入帳號 DNS Client 服務。  
  
 **影響：**的服務可能不會如預期般運作所需的權限。 如此一來，您可能會無法 DNS 名稱解析為伺服器。  
  
 **解析度：**  
  
##### <a name="to-change-the-dns-client-service-logon-account"></a>若要變更 DNS Client 服務登入 account  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**DNS Client**服務，並再按**屬性**。  
  
3.  在**登入**索引標籤，選取**此 account**，然後輸入**NT AUTHORITY\Network 服務**。  
  
### <a name="the-dns-server-service-should-use-the-local-system-account-as-its-logon-account"></a>DNS 伺服器服務應該使用本機系統 account 為其登入 account  
 **問題：**變更的 DNS 伺服器服務預設登入負責。  
  
 **影響：**的服務可能不會如預期般運作所需的權限。 如此一來，不可能會發生 DNS 更新。  
  
 **解析度：**  
  
##### <a name="to-change-the-dns-server-service-logon-account"></a>若要變更的 DNS 伺服器服務登入 account  
  
1.  在伺服器上的開放 services.msc  
  
2.  以滑鼠右鍵按一下**的 DNS 伺服器**服務，並再按**屬性**。  
  
3.  在**登入**索引標籤，選取**本機系統 account**。  
  
### <a name="active-directory-web-services-is-not-the-default-logon-account"></a>Active Directory 網頁的服務不預設登入 account  
 **問題：**未預設登入 account Active Directory 網頁的服務。 根據預設，登入 account 為**本機系統 account**。  
  
 **影響：**未開始 Active Directory Web 服務 (ADWS)。 如果在伺服器上的 ADWS 停止或停用，client 應用程式的 Windows PowerShell 模組 Active Directory 或 Active Directory 管理中心無法管理或存取此伺服器上執行 directory 服務執行個體。 如需詳細資訊，請查看[AD DS 中的新功能：Active Directory Web 服務](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx)(https://technet.microsoft.com/library/dd391908(WS.10).aspx) 在 Windows Server Technical 媒體櫃。  
  
 **解析度：**  
  
##### <a name="to-change-the-active-directory-web-services-logon-account"></a>若要變更登入帳號 Active Directory Web 服務  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**Active Directory Web 服務**，然後按**屬性**。  
  
3.  變更**開機類型**以**自動**，然後按一下 [ **[確定]**。  
  
4.  在 Active Directory Web 服務**屬性**，按一下 [**登入**索引標籤。  
  
5.  選取 [**本機系統 account**選項，然後按一下 [ **[確定]**。  
  
### <a name="the-windows-update-service-should-use-the-local-system-account-as-its-logon-account"></a>Windows Update 服務應該使用本機系統 account 為其登入 account  
 **問題：**變更的自動更新服務預設登入負責。  
  
 **影響：**的服務可能不會如預期般運作所需的權限。 如此一來，伺服器可能不會收到自動更新。  
  
 **解析度：**  
  
##### <a name="to-change-the-windows-update-service-logon-account"></a>若要變更 Windows Update 服務登入 account  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**Windows Update**服務，並再按屬性。  
  
3.  在**登入**索引標籤，選取**本機系統 account**。  
  
### <a name="the-dhcp-client-service-should-use-the-nt-authoritylocalservice-account-as-its-logon-account"></a>DHCP Client 服務應做為其登入 account 使用答 account  
 **問題：**變更的預設登入帳號 DHCP Client 服務。  
  
 **影響：**的服務可能不會如預期般運作所需的權限。 如此一來，client 電腦將會收到來自伺服器 IP 位址。  
  
 **解析度：**  
  
##### <a name="to-change-the-dhcp-client-service-logon-account"></a>若要變更 DHCP Client 服務登入 account  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**DHCP Client**服務，並再按**屬性**。  
  
3.  在**登入**索引標籤，選取**此 account**，然後輸入**NT AUTHORITY\Local 服務**。  
  
### <a name="the-iis-admin-service-should-use-the-local-system-account-as-its-logon-account"></a>IIS 管理服務應該使用本機系統 account 為其登入 account  
 **問題：**變更 IIS 管理服務預設登入負責。  
  
 **影響：**服務可能不會有所需的權限，才能如預期般運作。 如此一來，您可能無法存取伺服器，例如存取網路上執行的網站。  
  
 **解析度：**  
  
##### <a name="to-change-the-service-logon-account"></a>若要變更服務登入 account  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**IIS 管理服務**，然後按一下 [**屬性**。  
  
3.  在**登入**索引標籤，選取**本機系統 account**。  
  
### <a name="the-world-wide-web-publishing-service-should-use-the-local-system-account-as-its-logon-account"></a>全球發行服務應該使用本機系統 account 為其登入 account  
 **問題：**的全球發行服務預設登入 account 變更。  
  
 **影響：**的服務可能不會如預期般運作所需的權限。 如此一來，您可能無法存取伺服器，例如存取網路上執行的網站。  
  
 **解析度：**  
  
##### <a name="to-change-the-world-wide-web-publishing-service-logon-account"></a>若要變更的全球發行服務登入帳號  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**全球發行服務**，然後按**屬性**。  
  
3.  在**登入**索引標籤，選取**本機系統 account**。  
  
### <a name="the-remote-desktop-gateway-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>遠端桌面閘道服務應該使用 NT AUTHORITY\Network 服務 account 為其登入 account  
 **問題：**變更遠端桌面閘道服務預設登入負責。  
  
 **影響：**的服務可能無法如預期般運作的適當權限。 因此，使用者可能無法存取電腦使用遠端網路存取。  
  
 **解析度：**  
  
##### <a name="to-change-the-remote-desktop-gateway-service-logon-account"></a>若要變更遠端桌面閘道服務登入 account  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**遠端桌面閘道**服務，並再按**屬性**。  
  
3.  在**登入**索引標籤，選取**此 account**，然後輸入**NT AUTHORITY\Network 服務**。  
  
### <a name="the-windows-time-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Windows 時間服務應該使用 NT AUTHORITY\Network 服務 account 為其登入 account  
 **問題：** Windows 時間服務預設登入負責變更。  
  
 **影響：**的服務可能無法如預期般運作的適當權限。 如此一來的日期和時間同步可能不提供。  
  
 **解析度：**  
  
##### <a name="to-change-the-windows-time-service-logon-account"></a>若要變更 Windows 時間服務登入 account  
  
1.  打開 services.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下**Windows 時間**服務，並再按**屬性**。  
  
3.  在**登入**索引標籤，選取**此 account**，然後輸入**NT AUTHORITY\Local 服務**。  
  
### <a name="the-built-in-administrators-group-does-not-have-the-right-to-log-on-as-batch-job"></a>建的系統管理員群組不具有分批身分登入的權限  
 **問題：**系統管理員建群組不會有權分批身分登入。  
  
 **影響：**如果系統管理員會建立提醒，設定警示時的系統管理員未登入執行 2147943785 錯誤碼將會失敗警示。  
  
 **解析度：**如如何提供建的系統管理員身分分批登入的群組權限的相關資訊，[來登入為分批權限管理員建群組](https://technet.microsoft.com/library/jj635076)(https://technet.microsoft.com/library/jj635076)。  
  
### <a name="the-windows-firewall-is-turned-off"></a>關閉 Windows 防火牆  
 **問題：** Windows 防火牆已關閉。 預設值是上。  
  
 **影響：**根據您防火牆的設定，Windows 防火牆可協助保護您的伺服器及網路從惡意活動封鎖部分通過伺服器的資訊。  
  
 **解析度：**  
  
##### <a name="to-turn-on-windows-firewall-on-the-server"></a>將 Windows 防火牆伺服器上  
  
1.  打開伺服器上的 [控制台]。  
  
2.  在 [控制台] 中，按一下 [**系統及安全性**，然後按**Windows 防火牆**。  
  
3.  在 Windows 防火牆中，按一下 [**或關閉 Windows 防火牆**，請選取**Windows 防火牆關閉**選項，然後按一下 [ **[確定]**。  
  
### <a name="the-internal-network-adapter-is-not-configured-to-register-ip-address-in-dns"></a>連絡介面卡未設定為在 DNS 登記 IP 位址  
 **問題：**連絡介面卡未設定為在 DNS 登記 IP 位址。  
  
 **影響：**如果連絡介面卡的 IP 位址未係 DNS 中，它可能無法使用伺服器的電腦名稱來存取此伺服器。  
  
 **解析度：**確認連絡介面卡設定為在 DNS 登記。  
  
### <a name="dns-the-values-for-the-dns-forwardingtimeout-and-recursiontimeout-registry-key-are-identical"></a>DNS：值 DNS ForwardingTimeout 和 RecursionTimeout 登錄金鑰的相同  
 **問題：**的 DNS ForwardingTimeout 登錄鍵值不應該相同 RecursionTimeout 登錄按鍵的值。  
  
 **影響：**您無法存取網際網路資源依名稱。  
  
 **解析度：**設定的值為大於 ForwardingTimeout 按鍵 RecursionTimeout 登錄鍵，位於登錄 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters 的。  
  
### <a name="the-forward-dns-zone-for-your-active-directory-domain-does-not-allow-secure-updates"></a>您 Active Directory domain 向前 DNS 區域不允許安全性更新  
 **問題：**您應會設定向前對應區域允許安全的動態更新。  
  
 **影響：**當您讓安全的動態更新，只會使用授權的使用者並主機可以做的記錄。  
  
 **解析度：**  
  
##### <a name="to-configure-the-forward-lookup-zone-for-your-active-directory-domain"></a>若要設定的網域 Active Directory 正向對應區域  
  
1.  打開 dnsmgmt.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下您 Active Directory domain 的正向對應區域，然後按一下**屬性**。  
  
3.  在**動態更新**下拉式清單中，選取**只有安全**，然後按一下 [ **[確定]**。  
  
### <a name="the-forward-dns-zone-does-not-allow-secure-updates"></a>轉寄 DNS 區域不允許安全的更新  
 **問題：**您應會設定向前對應區域 _msdcs.* 區允許安全的動態更新。  
  
 **影響：**當您讓安全的動態更新，只會使用授權的使用者並主機可以做記錄 msdcs.* 區域。  
  
 **解析度：**  
  
##### <a name="to-allow-secure-updates-in-the-msdcs-zone"></a>若要允許 _msdcs 區域中的安全性更新  
  
1.  打開 dnsmgmt.msc 伺服器上。  
  
2.  以滑鼠右鍵按一下 _msdcs 區域，正向對應區域，然後按一下**屬性**。  
  
3.  在**動態更新**下拉式清單中，選取**只有安全**，然後按一下 [ **[確定]**。  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>不被支援 Internet Explorer 增強安全性設定  
 **問題：**適用於系統管理員群組目前不支援 Internet Explorer 增強安全性設定 (IE ESC)。  
  
 **影響：**系統管理員群組不支援 Internet Explorer 增強安全性設定，如果您的 Internet Explorer 和伺服器增加遭受惡意發動的攻擊可以透過內容和應用程式的指令碼。  
  
 **解析度：**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>若要讓 Internet Explorer 增強安全性設定  
  
1.  開放**伺服器管理員**上伺服器，然後按**本機伺服器**。  
  
2.  在**屬性**窗格中，變更的設定**IE 增強安全性設定**以**上**，，然後按一下**[確定]**。  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>不被支援 Internet Explorer 增強安全性設定  
 **問題：**適用於 Users 群組目前不支援 Internet Explorer 增強安全性設定 (IE ESC)。  
  
 **影響：**如果 Users 群組不支援 Internet Explorer 增強安全性設定，您的 Internet Explorer 和伺服器增加遭受惡意發動的攻擊可以透過內容和應用程式的指令碼。  
  
 **解析度：**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>若要讓 Internet Explorer 增強安全性設定  
  
1.  開放**伺服器管理員**，然後按**本機伺服器**。  
  
2.  在**屬性**窗格中，變更的設定**IE 增強安全性設定**以**上**，，然後按一下**[確定]**。  
  
### <a name="the-source-server-remains-in-active-directory-sites-and-services"></a>來源伺服器會留在 Active Directory 網站和服務  
 **問題：** Windows 小型企業 server 來源伺服器仍然在 Active Directory 網站和服務 Default-First-Site-Name。  
  
 **影響：**如果來源伺服器會留在主動式導演網站和服務，client 的電腦可能會遇到連接的問題：秒。  
  
 **解析度：**您應該會降級來源伺服器、將它移除網域中，從並再從 Active Directory 網站及服務和 Active Directory 使用者電腦 delete 來源伺服器。  
  
### <a name="source-server-remains-in-sbscomputer-ou"></a>來源伺服器會留在 SBSComputer 組織單位  
 **問題：** Windows 小型企業 server 來源伺服器仍然在 Active Directory 使用者與電腦。  
  
 **影響：**如果來源伺服器保持主動導演使用者和電腦，請 client 的電腦可能會遇到連接的問題：秒。  
  
 **解析度：**您應該會降級來源伺服器、將它移除網域中，從並再從 Active Directory 網站及服務和 Active Directory 使用者電腦 delete 來源伺服器。  
  
### <a name="a-group-policy-is-missing"></a>群組原則是遺失  
 **問題：**群組原則預設網域原則會遺失。  
  
 **影響：** [預設網域原則是正確的網域函式的。  
  
 **解析度：**  
  
##### <a name="to-restore-a-missing-group-policy"></a>若要還原遺失的群組原則  
  
1.  打開 gpmc.msc 伺服器上。  
  
2.  在群組原則管理員中，展開網域樹系並搜尋主機樹適用於**預設網域原則**群組原則物件。  
  
3.  如果未出現樹上的原則，從系統狀態備份還原。  
  
### <a name="no-dns-name-server-resource-records"></a>不 DNS 伺服器命名資源記錄  
 **問題：**在您的伺服器的正向對應區域中有不 DNS 名稱（奈秒）伺服器資源記錄。  
  
 **影響：**如果不 DNS 名稱（奈秒）伺服器資源記錄有正向對應的 Active Directory domain 區域，使用者可能會無法存取網路上或網際網路上的資源。  
  
 **解析度：**  
  
##### <a name="to-restore-missing-dns-name-server-resource-records"></a>若要還原遺失 DNS 伺服器命名資源記錄  
  
1.  打開 dnsmgmt.msc 伺服器上。  
  
2.  在 DNS 管理員正向對應區域的 Active Directory domain，以滑鼠右鍵按一下，然後按一下**屬性**。  
  
3.  在**名稱伺服器]**索引標籤上，請確認您的設定是否正確。  
  
4.  進行任何必要的變更，然後按一下**[確定]**以儲存的設定。  
  
### <a name="no-dns-name-server-records"></a>不 DNS 名稱伺服器記錄  
 **問題：**不有任何 DNS 名稱伺服器（奈秒）資源記錄伺服器 _msdcs 區域 (例如：_msdcs.contoso.local)。  
  
 **影響：**如果不有任何 DNS 名稱（奈秒）伺服器資源記錄 _msdcs 區域的 Active Directory domain，使用者可能會無法存取網路上或網際網路上的資源。  
  
 **解析度：**  
  
##### <a name="to-restore-missing-dns-name-server-records"></a>若要還原遺失的 DNS 名稱伺服器記錄  
  
1.  打開 dnsmgmt.msc 伺服器上。  
  
2.  在 DNS 管理員正向對應區域 _msdcs 區域，以滑鼠右鍵按一下，然後按一下**屬性**。  
  
3.  在**名稱伺服器]**索引標籤上，請確認您的設定是否正確。  
  
4.  進行任何必要的變更，然後按一下**[確定]**以儲存的設定。  
  
### <a name="no-dns-name-server-records"></a>不 DNS 名稱伺服器記錄  
 **問題：**不有任何 DNS 名稱伺服器（奈秒）資源的委派的 _msdcs 向前對應區域。  
  
 **影響：**如果委派的 _msdcs 向前對應區域中的任何 DNS 名稱（奈秒）伺服器資源記錄不的話 DNS 伺服器服務無法解析 DNS 網域資源記錄和將會失敗開始。  
  
 **解析度：**  
  
##### <a name="to-reconfigure-missing-dns-name-server-records"></a>若要重新設定遺失的 DNS 名稱伺服器記錄  
  
1.  打開 dnsmgmt.msc 伺服器上。  
  
2.  在 DNS 管理員中，依序展開您伺服器的名稱，和**正向對應區域**。  
  
3.  按一下 [為您的 Active Directory domain 正向對應區域 (例如：contoso.local)。  
  
4.  委派的 _msdcs 區域會出現灰色資料夾。 _Msdcs 區域，以滑鼠右鍵按一下，然後按一下**屬性**。  
  
5.  在**名稱伺服器]**索引標籤上，請確認您的設定是否正確。  
  
6.  進行任何必要的變更，然後按一下**[確定]**以儲存的設定。  
  
### <a name="authenticated-users-not-a-member-of-the-pre-windows-2000-compatible-access-group"></a>Windows 2000 相容存取群組成員已驗證的使用者  
 **問題：** Authenticated Users 群組不 windows 2000 相容存取群組成員。  
  
 **影響：**如果建 Authenticated Users 群組不是 windows 2000 相容存取群組成員，網路使用者可能會遇到「存取」錯誤。  
  
 **解析度：**  
  
##### <a name="to-add-authenticated-users-to-the-pre-windows-2000-compatible-access-group"></a>若要新增 Authenticated Users windows 2000 相容存取群組  
  
1.  打開 dsa.msc 伺服器上。  
  
2.  在**Builtin**資料夾，以滑鼠右鍵按一下**windows 2000 相容存取**，然後按一下 [**屬性**。  
  
3.  按一下**新增**，輸入**Authenticated Users**，然後按一下 [ **[確定]**兩次。  
  
### <a name="dns-client-not-configured"></a>未設定 DNS client  
 **問題：**未 DNS client 僅指向內部伺服器的 IP 位址設定。  
  
 **影響：**如果 DNS client 不只指向內部伺服器的 IP 位址設定，可能會失敗 DNS 名稱解析。  
  
 **解析度：**  
  
##### <a name="to-configure-dns-to-point-only-to-the-server-s-internal-ip-address"></a>若要設定的 DNS 伺服器 s 內部 IP 位址只指  
  
1.  從 client 的電腦，請打開**屬性**頁面上的網路。  
  
2.  請務必 DNS 已僅指向內部伺服器的 IP 位址設定。  
  
### <a name="default-application-pool-value-changed"></a>變更預設應用程式集區值  
 **問題：**未設定為預設值為 1 DefaultAppPool 應用程式集區的最大同事處理程序的數目。  
  
 **影響：**使用者可能會無法連接到 Windows 小型企業 Server 網頁的服務。  
  
 **解析度：**  
  
##### <a name="to-reset-maximum-worker-processes-for-the-default-application-pool"></a>預設應用程式集區重設最大的背景工作流程  
  
1.  打開伺服器管理員網際網路服務 (IIS)。  
  
2.  在 IIS 管理員中，展開 [伺服器名稱，然後按一下**應用程式集區**。  
  
3.  在**應用程式集區**，以滑鼠右鍵按一下**DefaultAppPool**，然後按一下 [**進階設定]**。  
  
4.  在**進階設定]**，變更的值為**最大的背景工作流程**1 並再按**[確定]**。  
  
5.  關閉**進階設定]**，以滑鼠右鍵按一下**DefaultAppPool**，然後停止及重新開機應用程式集區。  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-account"></a>遠端 Web 存取應用程式集區不會使用預設 account  
 **問題：**的預設 account 不執行 RemoteAppPool 應用程式集區。  
  
 **影響：**網路使用者可能就無法存取遠端網站存取的網站。  
  
 **解析度：**  
  
##### <a name="to-reset-the-remote-application-pool-to-use-the-default-account"></a>若要重設使用預設帳號遠端應用程式集  
  
1.  打開伺服器管理員網際網路服務 (IIS)。  
  
2.  在 IIS 管理員中，展開 [伺服器名稱，然後按一下**應用程式集區**。  
  
3.  在**應用程式集區**，以滑鼠右鍵按一下**RemoteAppPool**，然後按一下 [**進階設定]**。  
  
4.  在**進階設定]**，變更**身分**以**其他**，，然後按一下**[確定]**。  
  
5.  關閉**進階設定]**，以滑鼠右鍵按一下**RemoteAppPool**，然後停止及重新開機應用程式集區。  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-net-framework-version"></a>遠端 Web 存取應用程式集區不會使用預設的.NET Framework 版本  
 **問題：**使用預設的 Microsoft.NET Framework 的版本不執行 RemoteAppPool 應用程式集區。  
  
 **影響：**網路使用者可能就無法存取遠端網站存取的網站。  
  
 **解析度：**  
  
##### <a name="to-change-the-net-framework-version-used-by-the-remoteapppool"></a>若要變更 RemoteAppPool 所使用的.NET Framework 版本  
  
1.  打開伺服器管理員網際網路服務 (IIS)。  
  
2.  在 IIS 管理員中，展開 [伺服器名稱，然後按一下**應用程式集區**。  
  
3.  在**應用程式集區**，以滑鼠右鍵按一下**RemoteAppPool**，然後按一下 [**進階設定]**。  
  
4.  在**進階設定]**，變更**.NET Framework 版本**v4.0，並再按**[確定]**。  
  
5.  關閉**進階設定]**，以滑鼠右鍵按一下**RemoteAppPool**，然後停止及重新開機應用程式集區。  
  
### <a name="the-remoteaccesslog-file-is-larger-than-1-gb-in-size"></a>RemoteAccess.log 檔案大於 1 GB 的大小  
 **問題：**如果 Remoteaccess.log 檔案的大小超過 1 GB，您可以體驗少的磁碟空間錯誤系統磁碟機的問題。  
  
 **影響：**如果 Remoteaccess.log 檔案太大，可能會造成問題的可用空間：s c：磁碟機。  
  
 **解析度：**您備份伺服器之後，您可以 delete Remoteaccess.log 檔案，這位於 %ProgramData%\Microsoft\Windows Server\Logs\WebApps 資料夾。  
  
### <a name="default-web-sites-log-directory-is-over-1-gb-in-size"></a>預設網站的登入 directory 是超過 1 GB 的大小  
 **問題：**如果預設網站的登入資料夾的大小超過 1 GB，您可以體驗少的磁碟空間錯誤系統磁碟機的問題。  
  
 **影響：**如果預設網站的登入資料夾太大，可能會造成問題的可用空間：s c：磁碟機  
  
 **解析度：**您備份的伺服器，並雖然已停止預設的網站，您可以 delete 登入資料夾中的檔案 C:\inetpub\logs\LogFiles\W3SVC1 之後。 然後開始預設的網站。  
  
### <a name="no-binding-for-ssl-on-all-ip-addresses"></a>在所有的 IP 位址 SSL 不繫結  
 **問題：**上 [所有伺服器上的 IP 位址未繫結的安全通訊端層 (SSL)。  
  
 **影響：**如果伺服器上 SSL 不繫結至所有 IP 位址，某些網站將不會提供使用者使用。  
  
 **解析度：**  
  
##### <a name="to-bind-ssl-to-all-ip-addresses-on-the-server"></a>若要將 SSL 繫結到所有伺服器上的 IP 位址  
  
1.  打開伺服器管理員網際網路服務 (IIS)。  
  
2.  在 IIS 管理員中，在**連接**窗格中，依序展開伺服器、**網站**，以滑鼠右鍵按一下**預設網站**，，然後按一下**編輯繫結**。  
  
3.  在**網站繫結**，按一下 [**新增**，然後選取下列設定：  
  
    -   **輸入** = **https**  
  
    -   **IP 位址** = **全**  
  
    -   **連接埠**= 443  
  
4.  選取 SSL 憑證，，然後按一下**[確定]**儲存變更。  
  
### <a name="no-binding-for-ssl-on-the-default-web-site"></a>無預設網站上 SSL 繫結  
 **問題：**未繫結的 SSL 預設網站上。  
  
 **影響：**如果 SSL 不繫結至預設值的網站，某些網站可能無法提供使用者使用。  
  
 **解析度：**  
  
##### <a name="to-bind-ssl-to-the-default-website"></a>若要繫結 SSL 預設網站  
  
1.  打開伺服器管理員網際網路服務 (IIS)。  
  
2.  在 IIS 管理員中，在**連接**窗格中，依序展開伺服器、**網站**，以滑鼠右鍵按一下**預設網站**，，然後按一下**編輯繫結**。  
  
3.  在**網站繫結**，按一下 [**新增**，然後選取下列選項：  
  
    -   **輸入** = **https**  
  
    -   **IP 位址** = **全**  
  
    -   **連接埠**= 443  
  
    > [!NOTE]
    >  如果有一個特定的 IP 位址連接埠 443 HTTPS 繫結，變更**的 IP 位址**屬性該繫結至**全**。 例外是 127.0.0.1 的 IP 位址。 不會變更 127.0.0.1 繫結。  
  
4.  選取 SSL 憑證，，然後按一下**[確定]**儲存變更。  
  
### <a name="a-certificate-expires-within-30-days"></a>在 30 天後過期的憑證  
 **問題：**您伺服器的憑證將在 30 天後到期。  
  
 **影響：**伺服器無法使用過期的憑證。 如果過期的憑證，可能無法使用任何地方存取功能的使用者。  
  
 **解析度：**以防止過期的憑證，請更新您信任的憑證授權單位的憑證。  
  
### <a name="certificate-subject-does-not-match-the-name-configured-by-domain-name-wizard"></a>憑證主體名稱設定的網域名稱精靈不相符  
 **問題：**憑證主體名稱所設定的網域名稱精靈不相符。  
  
 **影響：**如果憑證主體名稱所設定的網域名稱精靈不符合，將不會初始化部分的網站。 其他網站將會顯示錯誤「還有此網站的安全性憑證有問題。」  
  
 **解析度：**以修正此問題的相關:，再次執行設定的任何地方存取精靈，並提供的認證，正確的網域名稱，或購買新的憑證符合您想要使用的網域名稱。  
  
### <a name="one-or-more-user-accounts-have-duplicate-cn-names"></a>一或多個帳號有重複 DATA-CN 名稱  
 **問題：**一或多個帳號有重複 DATA-CN 名稱：{0}。  
  
 **影響：**如果重複 DATA-CN 名稱帳號，使用者可能無法登入該網路。 此外，搜尋的 Active Directory 使用者可以退還不正確的值。  
  
 **解析度：**以修正此問題的相關:，確保網路帳號，不需要複製 [DATA-CN =」名稱。 這讓變得更容易，請考慮 Active Directory 內容匯出至檢視文字檔案。 有關如何執行此動作，請查看[使用的 LDIFDE 匯入及匯出 directory 物件 Active Directory（知識庫文章 237677）以](https://support.microsoft.com/kb/237677)(https://support.microsoft.com/kb/237677)。  
  
### <a name="nt-backup-is-installed"></a>安裝 NT 備份  
 **問題：** Windows NT 備份程式已安裝在伺服器上。  
  
 **影響：** Windows Server Essentials 使用 Windows Server 備份。 如果也已安裝 Windows NT 備份程式，可以衝突之間備份兩個程式。 這可能會造成 Windows Server 備份處理程序失敗。 衝突也可能會使您無法使用的備份還原伺服器。  
  
 **解析度：**以修正此問題的相關:，伺服器，NT 備份程式解除安裝。  
  
### <a name="iis-does-not-own-port-80-000080-or-port-443-0000443"></a>連接埠 80 (0.0.0.0:80) 或連接埠 (0.0.0.0:443) 443 IIS 不擁有  
 **問題：**未擁有連接埠 80 (0.0.0.0:80) 或連接埠 443 網際網路服務 (IIS)。 由其他應用程式目前結合這些連接埠。  
  
 **影響：** Windows Server Essentials web 應用程式需要使用連接埠 80 和連接埠 443，讓使用者提供的服務。 如果其他處理序或應用程式已使用連接埠 80 或連接埠 443，才能執行 Windows Server Essentials web 應用程式。 發生這種情形，如果遠端 Web Access 與其他應用程式會不提供使用者使用。  
  
 **解析度：**以修正此問題的相關:，請解除安裝的應用程式已使用連接埠 80 或連接埠 443，或指定不同的連接埠該應用程式。  
  
### <a name="the-default-website-is-not-running"></a>不執行預設的網站  
 **問題：**網站預設不執行 Windows Server Essentials 環境中。  
  
 **影響：** Windows Server Essentials web 應用程式必須使用預設的網站。 如果不執行預設的網站，並不適用於使用者遠端 Web Access 與其他應用程式。  
  
 **解析度：**  
  
##### <a name="to-start-the-default-website"></a>若要開始預設網站  
  
1.  打開伺服器管理員網際網路服務 (IIS)。  
  
2.  在 IIS 管理員中，展開 [伺服器名稱，然後按一下**網站**。  
  
3.  以滑鼠右鍵按一下**網站預設**，指向 [**管理網站**，，然後按一下 [ **[開始]**。  
  
### <a name="read-and-script-permissions-for-the-remote-virtual-directory-are-incorrect"></a>讀取和指令碼的權限 /Remote virtual directory 不正確  
 **問題：**讀取和指令碼權限不已指派給 /Remote virtual directory。  
  
 **影響：**如果讀取和指令碼權限的 /Remote virtual directory 不正確，使用者無法使用遠端 Web 存取。 當您嘗試使用存取的網頁瀏覽網際網路時，他們可能會遇到錯誤」HTTP 錯誤 403.1 禁止」。  
  
 **解析度：**  
  
##### <a name="to-assign-read-and-script-permissions-to-the-remote-directory"></a>若要將讀取和指令碼權限指派給 /Remote directory  
  
1.  打開伺服器管理員網際網路服務 (IIS)。  
  
2.  在 IIS 管理員中，展開 [伺服器名稱，然後按一下**網站**。  
  
3.  展開**網站預設**，然後展開**遠端**。  
  
4.  在**功能檢視**，按兩下 [**對應處理常式**。  
  
5.  在**動作**窗格中，按**編輯功能權限]**。  
  
6.  選取 [**朗讀**和**指令碼**核取方塊，並再按**[確定]**。  
  
### <a name="http-redirect-is-either-set-or-inherited-on-the-remote-virtual-directory"></a>HTTP 重新導向設定或繼承上 /Remote virtual directory  
 **問題：**意外設定或繼承上 /Remote virtual directory HTTP 重新導向屬性。  
  
 **影響：**如果 /Remote virtual directory 上設定 HTTP 重新導向屬性，遠端 Web 地點無法正常運作。  
  
 **解析度：**  
  
##### <a name="to-remove-the-http-redirect-attribute"></a>若要移除的 HTTP 重新導向屬性  
  
1.  打開伺服器管理員網際網路服務 (IIS)。  
  
2.  在 IIS 管理員中，展開 [伺服器名稱，然後按一下**網站**。  
  
3.  展開**網站預設**，然後展開**遠端**。  
  
4.  在**功能檢視**，按兩下 [ **HTTP 重新導向**。  
  
5.  清除**將這項目的要求**核取方塊，，然後按一下 [**套用**上**控制項**窗格。  
  
### <a name="a-host-name-exists-for-port-80-on-the-default-website"></a>連接埠 80 預設網站上存在的主機的名稱  
 **問題：**主機名稱指定連接埠 80 預設網站上。  
  
 **影響：**如果主機的名稱指定連接埠 80 預設網站上，您可能無法連接到部分的 Windows Server Essentials web 應用程式。 不需要主機的名稱，並建議您不要在這種情形  
  
 **解析度：**  
  
##### <a name="to-clear-the-host-name-entry-for-port-80-on-the-default-website"></a>若要清除的連接埠 80 預設網站上的主機名稱的項目  
  
1.  打開伺服器管理員網際網路服務 (IIS)。  
  
2.  在 IIS 管理員中，展開 [伺服器名稱，然後按一下**網站**。  
  
3.  在**功能檢視**，以滑鼠右鍵按一下**預設網站**，然後按一下 [**繫結**。  
  
4.  在**網站繫結**，請選取**的連接埠 80 http**設定，然後按**編輯**。  
  
5.  在**編輯網站繫結**，清除**主機名稱**的項目，然後再按一下**[確定]**。  
  
### <a name="backup-does-not-succeed-because-of-a-hidden-partition"></a>因為隱藏的磁碟分割備份未成功  
 **問題：**的非 NTFS 磁碟分割，Windows Server 備份排定的備份。  
  
 **影響：** Windows Server 備份只能備份磁碟分割格式化為 NTFS。  
  
 **解析度：**未設定 Windows Server 備份非 NTFS 磁碟分割備份。 如需詳細資訊，請查看[Windows Server 2008 的電腦（知識庫文章 968128）上的系統狀態備份失敗時登事件 Id 12290 和 16387](https://support.microsoft.com/kb/968128) (https://support.microsoft.com/kb/968128)。  
  
### <a name="the-most-recent-backup-did-not-succeed"></a>未成功的最近備份  
 **問題：**的最新的備份嘗試未成功。  
  
 **影響：**系統備份狀態不正確。  
  
 **解析度：**事件登和備份登的最近備份期間發生錯誤。  
  
### <a name="the-startup-type-for-the-file-replication-service-is-not-set-to-automatic"></a>未設定為 [自動開機類型的檔案複製服務  
 **問題：**如果開機類型未設定為預設值的自動，不可能會開始檔案複寫服務 (FRS)。  
  
 **影響：**的網域控制站如果檔案複寫服務無法執行，可能會停止廣告服務。 這可能會導致其他問題，例如登入的錯誤以及群組原則錯誤。  
  
 **解析度：**  
  
##### <a name="to-configure-the-file-replication-service-for-automatic-startup"></a>若要設定自動開機的檔案複製服務  
  
1.  打開服務主機。  
  
2.  在清單中的服務，按兩下 [**檔案複製**。  
  
3.  適用於**開機類型**，請選取**自動**，然後按一下 [**套用**。  
  
### <a name="the-file-replication-service-is-not-running"></a>不執行檔案複寫服務  
 **問題：**檔案複寫服務無法執行。  
  
 **影響：**的網域控制站如果檔案複寫服務無法執行，可能會停止廣告服務。 此問題可能會導致其他問題，例如登入的錯誤以及群組原則錯誤。  
  
 **解析度：**  
  
##### <a name="to-start-the-file-replication-service"></a>若要開始檔案複寫服務  
  
1.  打開服務主機。  
  
2.  在清單中的服務，按兩下 [**檔案複寫服務**。  
  
3.  按一下**[開始]**。  
  
### <a name="the-logon-account-for-the-file-replication-service-is-not-set-to-use-the-local-system-account"></a>使用本機系統 account 未設定檔複寫服務負責登入  
 **問題：**未設定為預設登入帳號使用本機系統 account 檔案複寫服務。  
  
 **影響：**如果檔案複寫服務不使用本機系統為預設登入帳號，您可能會遇到的權限相關的錯誤。 這些錯誤可以觸發其他錯誤，以及最終可能造成網域控制站停止廣告服務。  
  
 **解析度：**  
  
##### <a name="to-configure-local-system-as-the-default-logon-account-for-file-replication"></a>若要設定為預設登入帳號本機系統檔案複寫  
  
1.  打開服務主機。  
  
2.  在清單中的服務，按兩下 [**檔案複製**。  
  
3.  在**服務屬性**頁面上，按**登入**索引標籤。  
  
4.  選取 [**本機系統 account**選項，然後按一下 [**套用]**。  
  
5.  重新開機服務。  
  
### <a name="the-startup-type-for-the-dfs-replication-service-is-not-set-to-automatic"></a>未設定為 [自動 DFS 複寫服務開機類型  
 **問題：**如果開機類型未設定為預設值的自動，不可能會開始 DFS 複寫服務。  
  
 **影響：**的網域控制站如果 DFS 複寫服務無法執行，可能會停止廣告服務。 這可能會導致其他問題，例如登入的錯誤以及群組原則錯誤。  
  
 **解析度：**  
  
##### <a name="to-configure-the-dfs-replication-service-for-automatic-startup"></a>若要設定為自動開機 DFS 複寫服務  
  
1.  打開服務主機。  
  
2.  在清單中的服務，按兩下 [ **DFS 複製**。  
  
3.  適用於**開機類型**，請選取**自動**，然後按一下 [**套用**。  
  
### <a name="the-dfs-replication-service-is-not-running"></a>不執行 DFS 複寫服務  
 **問題：** DFS 複寫服務目前無法執行。  
  
 **影響：**的網域控制站如果 DFS 複寫服務無法執行，可能會停止廣告服務。 此問題可能會導致其他問題，例如登入的錯誤以及群組原則錯誤。  
  
 **解析度：**  
  
##### <a name="to-start-the-dfs-replication-service"></a>若要開始 DFS 複寫服務  
  
1.  打開服務主機。  
  
2.  在清單中的服務，按兩下 [ **DFS 複製**。  
  
3.  按一下**[開始]**。  
  
### <a name="the-dfs-replication-service-is-not-is-not-set-to-use-the-local-system-account"></a>服務不 DFS 複寫不是設為使用本機系統 account  
 **問題：**未設定為預設登入帳號使用本機系統 account DFS 複寫服務。  
  
 **影響：**如果 DFS 複寫服務不使用本機系統為預設登入帳號，您可能會遇到的權限相關的錯誤。 這些錯誤可以觸發其他錯誤，以及最終可能造成網域控制站停止廣告服務。  
  
 **解析度：**  
  
##### <a name="to-configure-dfs-replication-to-use-local-system-as-the-default-logon-account"></a>若要設定為預設登入帳號使用本機系統 DFS 複寫  
  
1.  打開服務主機。  
  
2.  在清單中的服務，按兩下 [ **DFS 複製**。  
  
3.  在**服務屬性**頁面上，按**登入**索引標籤。  
  
4.  選取 [**本機系統 account**選項，然後按一下 [**套用]**。  
  
5.  重新開機服務。  
  
### <a name="the-windows-server-office-365-integration-service-is-not-set-to-use-the-local-system-account"></a>若要使用本機系統 account 未設定 Windows Server Office 365 整合服務  
 **問題：**未設定為預設登入帳號使用本機系統 account Windows 伺服器 Office 365 整合服務。  
  
 **影響：**如果 Windows 伺服器 Office 365 整合服務不使用本機系統為預設登入帳號，Office 365 的部分功能可能無法正確運作。 您也可能會遇到的權限相關的錯誤。  
  
 **解析度：**  
  
##### <a name="to-configure-the-office-365-integration-service-to-use-local-system-as-the-default-logon-account"></a>若要設定為預設登入帳號使用本機系統 Office 365 整合服務  
  
1.  打開服務主機。  
  
2.  在清單中的服務，按兩下 [ **Windows 伺服器 Office 365 整合服務**。  
  
3.  在**服務屬性**頁面上，按**登入**索引標籤。  
  
4.  選取 [**本機系統 account**選項，然後按一下 [**套用]**。  
  
5.  重新開機服務。  
  
### <a name="the-windows-server-office-365-integration-service-is-not-running"></a>不執行 Windows Server Office 365 整合服務  
 **問題：**未目前正在執行 Windows Server Office 365 整合服務。  
  
 **影響：**如果不執行 Windows Server Office 365 整合服務，以雲端為基礎的 Office 365 功能則無法使用。  
  
 **解析度：**  
  
##### <a name="to-start-the-windows-server-office-365-integration-service"></a>若要開始 Windows 伺服器 Office 365 整合服務  
  
1.  打開服務主機。  
  
2.  在清單中的服務，按兩下 [ **Windows 伺服器 Office 365 整合服務**。  
  
3.  按一下**[開始]**。  
  
### <a name="the-startup-type-for-the-windows-server-office-365-integration-service-is-not-set-to-automatic"></a>未設定為 [自動開機的 Windows Server Office 365 整合服務類型  
 **問題：**如果開機類型未設定為預設值的自動，不可能會開始 Windows 伺服器 Office 365 整合服務。  
  
 **影響：**如果不執行 Windows Server Office 365 整合服務，以雲端為基礎的 Office 365 功能則無法使用。  
  
 **解析度：**  
  
##### <a name="to-configure-the-office-365-integration-service-for-automatic-startup"></a>若要設定為自動開機 Office 365 整合服務  
  
1.  打開服務主機。  
  
2.  在清單中的服務，按兩下 [ **Windows 伺服器 Office 365 整合服務**。  
  
3.  適用於**開機類型**，請選取**自動**，然後按一下 [**套用**。  
  
### <a name="a-registry-value-is-missing-or-set-incorrectly"></a>登錄價值，是遺失或不正確設定  
 **問題：**在跳 \Software\Microsoft\Rpc\RpcProxy 登錄按鍵包含不正確的值，或不存在。  
  
 **影響：**如果 RPCProxy 登錄鍵設定不正確，您可能會收到類似下列錯誤訊息:「因為遠端桌面閘道伺服器暫時無法使用，您的電腦無法連接到遠端電腦。 請稍後再重新連接，或您的網路系統管理員尋求協助。」  
  
 **解析度：**  
  
##### <a name="to-correct-the-registry-setting"></a>若要修正的登錄設定  
  
1.  打開作業系統。  
  
2.  瀏覽至下列機碼：  
  
     HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy  
  
3.  確保名為「網站「字串有預設的網站資料值：  
  
    -   如果不正確資料值，修改字串使用正確的值。  
  
    -   如果字串不存在，建立新的字串名為「網站」和資料值設成預設值的網站。」  
  
### <a name="the-startup-type-for-the-block-level-backup-engine-service-is-not-set-to-manual"></a>未設定為手動開機的 [封鎖層級備份引擎服務類型  
 **問題：**封鎖層級備份引擎服務不使用手動預設開機類型。  
  
 **影響：**封鎖層級備份引擎服務，不可能會開始如果開機類型不設定為手動。 此問題：可能會造成 Windows Server 備份工作失敗。  
  
 **解析度：**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-for-manual-startup"></a>若要手動開機將封鎖層級備份引擎服務設定  
  
1.  打開服務主機。  
  
2.  在清單中的服務，按兩下 [**封鎖層級備份引擎服務**。  
  
3.  適用於**開機類型**，請選取**手動**，然後按一下 [**套用**。  
  
### <a name="the-logon-account-for-the-block-level-backup-engine-service-is-not-set-to-use-the-local-system-account"></a>登入負責封鎖層級備份引擎服務不是設定為使用本機系統 account  
 **問題：**未設定為預設登入帳號使用本機系統 account 封鎖層級備份引擎服務。  
  
 **影響：**如果封鎖層級備份引擎服務不使用本機系統為預設登入帳號，您可能會遇到的權限相關的錯誤。 這些錯誤可以阻止 Windows Server 備份工作成功完成。  
  
 **解析度：**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-to-use-local-system-as-the-default-logon-account"></a>設定為預設登入帳號使用本機系統封鎖層級備份引擎服務  
  
1.  打開服務主機。  
  
2.  在清單中的服務，按兩下 [**封鎖層級備份引擎服務**。  
  
3.  在**服務屬性**頁面上，按**登入**索引標籤。  
  
4.  選取 [**本機系統 account**選項，然後按一下 [**套用]**。  
  
5.  重新開機服務。  
  
### <a name="the-common-name-on-the-certificate-that-is-bound-to-the-wss-certificate-web-service-website-does-not-match-the-server-name"></a>不符伺服器名稱憑證繫結至 WSS 憑證 Web 服務的網站上的一般的名稱。  
 **問題：**在 WSS 憑證 Web 服務網站繫結非有效的憑證。 在此憑證的一般的名稱不符伺服器名稱。  
  
 **影響：**如果您將非有效的憑證繫結至 WSS 憑證 Web 服務網站連接精靈可能無法正常運作。  
  
 **解析度：**  
  
##### <a name="to-configure-a-valid-certificate-for-the-wss-certificate-web-service"></a>若要設定有效的憑證 WSS 憑證 Web 服務  
  
1.  打開伺服器管理員網際網路服務 (IIS)。  
  
2.  在 IIS 管理員中，展開 [伺服器名稱，然後按一下**網站**。  
  
3.  以滑鼠右鍵按一下**WSS 憑證 Web 服務**，然後按**編輯繫結**。  
  
4.  在**網站繫結**，按一下 [ **HTTPS**，然後按一下 [**編輯**。  
  
5.  在**編輯網站繫結**，為**SSL 憑證**，選取 [伺服器名稱相同的憑證。  
  
6.  如果多個憑證項目有相同的名稱為您的伺服器，請按一下**檢視**來判斷哪一個憑證有效，然後選取適當的憑證。  
  
### <a name="there-appears-to-be-a-problem-with-the-certificate-binding-for-the-remote-desktop-gateway-service"></a>似乎那里遠端桌面閘道服務的憑證繫結的問題  
 **問題：**結合錯誤似乎遠端桌面閘道服務的憑證。  
  
 **影響：**如果憑證遠端桌面閘道服務設定不正確，使用者無法連接到遠端的網路存取權。  
  
 **解析度：**  
  
##### <a name="to-fix-the-binding-for-the-remote-desktop-gateway-service"></a>若要修正遠端桌面閘道服務的繫結  
  
-   打開系統管理員的身分、為命令提示字元中，輸入下列命令：  
  
    ```  
    REG ADD HKLM\SYSTEM\CurrentControlSet\services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443  /v DefaultFlags /t REG_DWORD /d 1 /f  
    net stop tsgateway  
    net start tsgateway  
    ```  
  
     如需詳細資訊，請查看[如何管理遠端桌面閘道服務，Windows Server Essentials（知識庫文章 2472211）在](https://support.microsoft.com/kb/2472211)(https://support.microsoft.com/kb/2472211)。
