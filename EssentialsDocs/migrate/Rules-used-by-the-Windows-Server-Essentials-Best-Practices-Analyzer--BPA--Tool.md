---
title: Windows Server Essentials 最佳做法分析器 (BPA) 工具所使用的規則
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 37e1dae7-586c-4dd7-bf83-7e14a9567c8f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: eea437a5867a602a84483a41fe129d64425bcb88
ms.sourcegitcommit: 04637054de2bfbac66b9c78bad7bf3e7bae5ffb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87838437"
---
# <a name="rules-used-by-the-windows-server-essentials-best-practices-analyzer-bpa-tool"></a>Windows Server Essentials 最佳做法分析器 (BPA) 工具所使用的規則

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

本文說明 Windows Server Essentials 最佳做法分析程式 (BPA) 所使用的規則。 BPA 會檢查正在執行 Windows Server Essentials 的伺服器，並顯示描述問題的報告，並提供解決它們的建議。 建議是由適用于 Windows Server Essentials 的產品支援組織所開發。

## <a name="using-the-tool"></a>使用此工具
 這是標準的作法，當您從 Windows Server 2011 Essentials、Windows Small Business Server 2011 Essentials 或 Windows Home Server 2011 遷移至 Windows Server Essentials 時，若要在完成遷移設定和資料之後，在目的地伺服器上執行 BPA。 您可以隨時從儀表板執行此工具。

#### <a name="to-run-the--windows-server-essentials-bpa-on-the-server"></a>在伺服器上執行 Windows Server Essentials BPA

1.  以系統管理員身分登入伺服器，然後開啟 [儀表板]。

2.  在儀表板上，按一下 [裝置]**** 索引標籤。

3.  在 **[伺服器工作]** 窗格中，按一下 [最佳做法分析程式]****。

4.  如有必要，請檢閱每個 BPA 訊息，並遵循指示來解決問題。

## <a name="rules-used-by-the-best-practices-analyzer"></a>最佳做法分析程式所使用的規則

### <a name="disable-ip-filtering"></a>停用 IP 篩選
 **問題：** 伺服器上目前已啟用 IP 篩選。 您必須停用 IP 篩選。

 **影響：** 如果已啟用 IP 篩選，網路流量可能會遭到封鎖。

 **解決方案：**

##### <a name="to-disable-ip-filtering"></a>停用 IP 篩選

1.  在伺服器上開啟 regedit.exe。

2.  巡覽至 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters。

3.  以滑鼠右鍵按一下 [EnableSecurityFilters]****，然後按一下 [修改]****。

4.  在 **[編輯 DWORD (32 位元) 值]** 視窗中，將 **[數值資料]** 欄位變更為零，然後按一下 [確定]****。

5.  重新啟動伺服器以套用變更。

### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-set-to-start-automatically-by-default"></a>分散式交易協調器 (MSDTC) 服務應該設定為依預設自動啟動
 **問題：** MSDTC 服務未設定為自動啟動

 **影響：** 當伺服器啟動時，MSDTC 服務可能不會自動啟動。 如果此服務停止，某些 SQL Server 或 COM 函數可能會失敗。 如此一來，使用 Microsoft SQL Server 或 COM 函數的應用程式可能無法正確運作。

 **解決方案：**

##### <a name="to-configure-the-msdtc-service-to-start-automatically"></a>將 MSDTC 服務設定為自動啟動

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [**分散式交易協調器**] 服務，然後按一下 [**內容**]。

3.  在 [一般]**** 索引標籤上，將 [啟動類型]**** 變更為 [自動 (延遲開始)]****，然後按一下 [確定]****。

### <a name="the-netlogon-service-should-be-configured-to-start-automatically-by-default"></a>Netlogon 服務應該設定為依預設自動啟動
 **問題：** Netlogon 服務未設定為自動啟動。

 **影響：** 當伺服器啟動時，Netlogon 服務可能不會自動啟動。 如果此服務停止，伺服器可能不會驗證使用者和服務。

 **解決方案：**

##### <a name="to-configure-the-netlogon-service-to-start-automatically"></a>將 Netlogon 服務設定為自動啟動

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [Netlogon]**** 服務，然後按一下 [屬性]****。

3.  在 [一般]**** 索引標籤上，將 [啟動類型]**** 變更為 [自動]****，然後按一下 [確定]****。

### <a name="the-dns-client-service-should-be-configured-to-start-automatically-by-default"></a>DNS 用戶端服務應該設定為依預設自動啟動
 **問題：** DNS 用戶端服務未設定為自動啟動。

 **影響：** 當伺服器啟動時，DNS 用戶端服務可能不會自動啟動。 如果此服務停止，伺服器可能無法解析 DNS 名稱。

 **解決方案：**

##### <a name="to-configure-the-dns-client-service-to-start-automatically"></a>將 DNS 用戶端服務設定為自動啟動

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [DNS 用戶端]**** 服務，然後按一下 [屬性]****。

3.  在 [一般]**** 索引標籤上，將 [啟動類型]**** 變更為 [自動]****，然後按一下 [確定]****。

### <a name="the-dns-server-service-should-be-configured-to-start-automatically-by-default"></a>DNS 伺服器服務應該設定為依預設自動啟動
 **問題：** DNS 伺服器服務未設定為自動啟動。

 **影響：** 當伺服器啟動時，DNS 伺服器服務可能不會自動啟動。 如果此服務停止，則不會進行 DNS 更新。

 **解決方案：**

##### <a name="to-configure-the-dns-server-service-to-start-automatically"></a>將 DNS 伺服器服務設定為自動啟動

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [DNS 伺服器]**** 服務，然後按一下 [屬性]****。

3.  在 [一般]**** 索引標籤上，將 [啟動類型]**** 變更為 [自動]****，然後按一下 [確定]****。

### <a name="active-directory-web-services-is-not-set-to-the-default-start-mode"></a>Active Directory Web 服務未設定為預設的啟動模式
 **問題：** Active Directory Web 服務未設定為預設的自動啟動模式。

 **影響：** Active Directory Web 服務 (ADWS) 未設定為預設的自動啟動模式。 如果伺服器上的 ADWS 已停止或停用，則 Windows PowerShell 的 Active Directory 模組或 Active Directory 管理中心這類的用戶端應用程式，就無法存取或管理在此伺服器上執行的目錄服務執行個體。 如需詳細資訊，請參閱 Windows Server 技術文件庫中[AD DS 的新功能： Active Directory Web 服務](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) 。

 **解決方案：**

##### <a name="to-configure-the-active-directory-web-services-service-to-start-automatically"></a>將 Active Directory Web 服務的服務設定為自動啟動

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [Active Directory Web 服務]**** 服務，然後按一下 [屬性]****。

3.  在 [一般]**** 索引標籤上，將 [啟動類型]**** 變更為 [自動]****，然後按一下 [確定]****。

### <a name="the-dhcp-client-service-should-be-configured-to-start-automatically-by-default"></a>DHCP 用戶端服務應該設定為依預設自動啟動
 **問題：** DHCP 用戶端服務未設定為自動啟動。

 **影響：** 當伺服器啟動時，DHCP 用戶端服務將不會自動啟動。 如果此服務停止，則用戶端電腦無法從伺服器接收 IP 位址。

 **解決方案：**

##### <a name="to-configure-the-dhcp-client-service-to-start-automatically"></a>將 DHCP 用戶端服務設定為自動啟動

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [DHCP 用戶端]**** 服務，然後按一下 [屬性]****。

3.  在 [一般]**** 索引標籤上，將 [啟動類型]**** 變更為 [自動]****，然後按一下 [確定]****。

### <a name="the-iis-admin-service-should-be-configured-to-start-automatically-by-default"></a>IIS Admin Service 應該設定為依預設自動啟動
 **問題：** IIS 管理服務未設定為自動啟動。

 **影響：** 當伺服器啟動時，IIS Admin 服務不會自動啟動。 如果此服務停止，則您可能無法存取在此伺服器上執行的網站，例如遠端 Web 存取。

 **解決方案：**

##### <a name="to-configure-the-iis-admin-service-to-start-automatically"></a>將 IIS Admin Service 設定為自動啟動

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [IIS Admin Service]****，然後按一下 [屬性]****。

3.  在 [一般]**** 索引標籤上，將 [啟動類型]**** 變更為 [自動]****，然後按一下 [確定]****。

### <a name="the-world-wide-web-publishing-service-should-be-configured-to-start-automatically-by-default"></a>World Wide Web Publishing 服務應該設定為依預設自動啟動
 **問題：** World Wide Web 發行服務未設定為自動啟動。

 **影響：** 當伺服器啟動時，World Wide Web 發行服務可能不會自動啟動。 如果此服務停止，則您可能無法存取在此伺服器上執行的網站，例如遠端 Web 存取。

 **解決方案：**

##### <a name="to-configure-the-world-wide-web-publishing-service-to-start-automatically"></a>將 World Wide Web Publishing 服務設定為自動啟動

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [World Wide Web Publishing 服務]****，然後按一下 [屬性]****。

3.  在 [一般]**** 索引標籤上，將 [啟動類型]**** 變更為 [自動]****，然後按一下 [確定]****。

### <a name="the-remote-registry-service-should-be-configured-to-start-automatically-by-default"></a>遠端登錄服務應該設定為依預設自動啟動
 **問題：** 遠端登入服務未設定為自動啟動。

 **影響：**

 當伺服器啟動時，遠端登錄服務可能不會自動啟動。 如果此服務停止，您可能無法從遠端執行某些網路作業。

 **解決方案：**

##### <a name="to-configure-the-remote-registry-service-to-start-automatically"></a>將遠端登錄服務設定為自動啟動

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [遠端登錄]**** 服務，然後按一下 [屬性]****。

3.  在 [一般]**** 索引標籤上，將 [啟動類型]**** 變更為 [自動]****，然後按一下 [確定]****。

### <a name="the-remote-desktop-gateway-service-should-be-configured-to-start-automatically-by-default"></a>遠端桌面閘道服務應該設定為依預設自動啟動
 **問題：** 遠端桌面閘道服務未設定為自動啟動。

 **影響：** 如果此服務停止，使用者可能無法使用遠端 Web 存取來存取電腦。

 **解決方案：**

##### <a name="to-configure-the-remote-desktop-gateway-service-to-start-automatically"></a>將遠端桌面閘道服務設定為自動啟動

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [遠端桌面閘道]**** 服務，然後按一下 [屬性]****。

3.  在 [一般]**** 索引標籤上，將 [啟動類型]**** 變更為 [自動 (延遲開始)]****，然後按一下 [確定]****。

### <a name="the-windows-time-service-should-be-configured-to-start-automatically-by-default"></a>Windows 時間服務應該設定為依預設自動啟動
 **問題：** Windows 時間服務未設定為自動啟動。

 **影響：** 如果此服務停止，則無法使用資料和時間同步處理。

 **解決方案：**

##### <a name="to-configure-the-windows-time-service-to-start-automatically"></a>將 Windows 時間服務設定為自動啟動

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [Windows 時間]**** 服務，然後按一下 [屬性]****。

3.  在 [一般]**** 索引標籤上，將 [啟動類型]**** 變更為 [自動]****，然後按一下 [確定]****。

### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-started"></a>分散式交易協調器 (MSDTC) 服務應該要啟動
 **問題：** MSDTC 服務未在伺服器上執行。

 **影響：** 如果此服務停止，部分 SQL Server 或 COM 函數可能會失敗。 如此一來，使用 Microsoft SQL Server 或 COM 函數的應用程式可能無法正確運作。

 **解決方案：**

##### <a name="to-start-the-distributed-transaction-coordinator-service"></a>若要啟動分散式交易協調器服務

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [分散式交易協調器]**** 服務，然後按一下 [啟動]****。

### <a name="the-netlogon-service-should-be-started"></a>Netlogon 服務應該要啟動
 **問題：** Netlogon 服務未在伺服器上執行。

 **影響：** 如果此服務未啟動，伺服器可能不會驗證使用者和服務。

 **解決方案：**

##### <a name="to-start-the-netlogon-service"></a>啟動 Netlogon 服務

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [Netlogon]**** 服務，然後按一下 [啟動]****。

### <a name="the-dns-client-service-should-be-started"></a>DNS 用戶端服務應該要啟動
 **問題：** DNS 用戶端服務未在伺服器上執行。

 **影響：** 如果此服務未啟動，伺服器可能無法解析 DNS 名稱。

 **解決方案：**

##### <a name="to-start-the-dns-client-service"></a>啟動 DNS 用戶端服務

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [DNS 用戶端]**** 服務，然後按一下 [啟動]****。

### <a name="the-dns-server-service-should-be-started"></a>DNS 伺服器服務應該要啟動
 **問題：** DNS 伺服器服務未在伺服器上執行。

 **影響：** 如果 DNS 伺服器服務未啟動，可能不會進行 DNS 更新。

 **解決方案：**

##### <a name="to-start-the-dns-server-service"></a>啟動 DNS 伺服器服務

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [DNS 伺服器]**** 服務，然後按一下 [啟動]****。

### <a name="active-directory-web-services-is-not-started"></a>Active Directory Web 服務未啟動
 **問題：** Active Directory Web 服務未啟動。

 **影響：** 未啟動 Active Directory Web 服務 (ADWS) 。 如果伺服器上的 ADWS 已停止或停用，則 Windows PowerShell 的 Active Directory 模組或 Active Directory 管理中心這類的用戶端應用程式，就無法存取或管理在此伺服器上執行的目錄服務執行個體。 如需詳細資訊，請參閱 Windows Server 技術文件庫中[AD DS 的新功能： Active Directory Web 服務](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) 。

 **解決方案：**

##### <a name="to-start-the-active-directory-web-services-service"></a>啟動 Active Directory Web 服務的服務

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [Active Directory Web 服務]****，然後按一下 [啟動]****。

### <a name="the-dhcp-client-service-should-be-started"></a>DHCP 用戶端服務應該要啟動
 **問題：** DHCP 用戶端服務未在伺服器上執行。

 **影響：** 如果停止此服務，用戶端電腦就無法從伺服器接收 IP 位址。

 **解決方案：**

##### <a name="to-start-the-dhcp-client-service"></a>啟動 DHCP 用戶端服務

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [DHCP 用戶端]**** 服務，然後按一下 [啟動]****。

### <a name="the-iis-admin-service-should-be-started"></a>IIS Admin Service 應該要啟動
 **問題：** IIS 管理服務並未在伺服器上執行。

 **影響：** 如果此服務停止，您可能無法存取在伺服器上執行的網站，例如遠端 Web 存取。

 **解決方案：**

##### <a name="to-start-the-iis-admin-service"></a>啟動 IIS Admin Service。

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [IIS Admin Service]****，然後按一下 [啟動]****。

### <a name="the-world-wide-web-publishing-service-should-be-started"></a>World Wide Web Publishing 服務應該要啟動。
 **問題：** World Wide Web 發行服務並未在伺服器上執行。

 **影響：** 如果此服務停止，您可能無法存取在伺服器上執行的網站，例如遠端 Web 存取。

 **解決方案：**

##### <a name="to-start-the-world-wide-web-publishing-service"></a>啟動 World Wide Web Publishing 服務

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [World Wide Web Publishing 服務]****，然後按一下 [啟動]****。

### <a name="the-remote-desktop-gateway-service-should-be-started"></a>遠端桌面閘道服務應該要啟動
 **問題：** 伺服器上未執行遠端桌面閘道服務。

 **影響：** 如果此服務停止，使用者可能無法使用遠端 Web 存取來存取電腦。

 **解決方案：**

##### <a name="to-start-the-remote-desktop-gateway-service"></a>啟動遠端桌面閘道服務

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [遠端桌面閘道]**** 服務，然後按一下 [啟動]****。

### <a name="the-windows-time-service-should-be-started"></a>Windows 時間服務應該要啟動
 **問題：** Windows 時間服務並未在伺服器上執行。

 **影響：** 如果停止此服務，將無法使用資料和時間同步處理。

 **解決方案：**

##### <a name="to-start-the-windows-time-service"></a>若要啟動 Windows Time 服務

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [Windows 時間]**** 服務，然後按一下 [啟動]****。

### <a name="the-distributed-transaction-coordinator-msdtc-service-logon-account-should-be-nt-authoritynetwork-service"></a>分散式交易協調器 (MSDTC) 服務登入帳戶應該是 NT AUTHORITY\Network Service
 **問題：** 分散式交易協調器 (MSDTC) 服務的預設登入帳戶已變更。

 **影響：** 服務可能沒有如預期般運作所需的許可權。 如此一來，使用 SQL Server 或 COM 函數的應用程式可能無法正確運作。

 **解決方案：**

##### <a name="to-change-the-logon-account-for-the-service"></a>變更服務的登入帳戶

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [**分散式交易協調器**] 服務，然後按一下 [**內容**]。

3.  在 [登入]**** 索引標籤中，選取 [此帳戶]****，輸入 **NT AUTHORITY\Network Service**，然後按一下 [確定]****。

### <a name="the-netlogon-service-should-use-the-local-system-account-as-its-logon-account"></a>Netlogon 服務應使用本機系統帳戶做為其登入帳戶
 **問題：** Netlogon 服務的預設登入帳戶已變更。

 **影響：** 服務可能沒有如預期般運作所需的許可權。 如此一來，伺服器可能不會驗證使用者與服務。

 **解決方案：**

##### <a name="to-change-the-netlogon-service-logon-account"></a>變更 Netlogon 服務登入帳戶

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [Netlogon]**** 服務，然後按一下 [屬性]****。

3.  在 [登入]**** 索引標籤中選取 [本機系統帳戶]****。

### <a name="the-dns-client-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>DNS 用戶端服務應該使用 NT AUTHORITY\Network Service 帳戶做為其登入帳戶
 **問題：** DNS 用戶端服務的預設登入帳戶已變更。

 **影響：** 服務可能沒有如預期般運作所需的許可權。 如此一來，伺服器可能無法解析 DNS 名稱。

 **解決方案：**

##### <a name="to-change-the-dns-client-service-logon-account"></a>變更 DNS 用戶端服務登入帳戶

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [DNS 用戶端]**** 服務，然後按一下 [屬性]****。

3.  在 [登入]**** 索引標籤中，選取 [此帳戶]****，然後輸入 **NT AUTHORITY\Network Service**。

### <a name="the-dns-server-service-should-use-the-local-system-account-as-its-logon-account"></a>DNS 伺服器服務應該使用本機系統帳戶做為其登入帳戶
 **問題：** DNS 伺服器服務的預設登入帳戶已變更。

 **影響：** 服務可能沒有如預期般運作所需的許可權。 如此一來，可能不會進行 DNS 更新。

 **解決方案：**

##### <a name="to-change-the-dns-server-service-logon-account"></a>變更 DNS 伺服器服務登入帳戶

1.  在伺服器上開啟 services.msc

2.  以滑鼠右鍵按一下 [DNS 伺服器]**** 服務，然後按一下 [屬性]****。

3.  在 [登入]**** 索引標籤中選取 [本機系統帳戶]****。

### <a name="active-directory-web-services-is-not-the-default-logon-account"></a>Active Directory Web 服務不是預設登入帳戶
 **問題：** Active Directory Web 服務不是預設登入帳戶。 根據預設，登入帳戶設定為**本機系統帳戶**。

 **影響：** 未啟動 Active Directory Web 服務 (ADWS) 。 如果伺服器上的 ADWS 已停止或停用，則 Windows PowerShell 的 Active Directory 模組或 Active Directory 管理中心這類的用戶端應用程式，就無法存取或管理在此伺服器上執行的目錄服務執行個體。 如需詳細資訊，請參閱 Windows Server 技術文件庫中[AD DS 的新功能： Active Directory Web 服務](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) 。

 **解決方案：**

##### <a name="to-change-the-active-directory-web-services-logon-account"></a>變更 Active Directory Web 服務登入帳戶

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [Active Directory Web 服務]****，然後按一下 [屬性]****。

3.  將 [啟動類型]**** 變更為 [自動]****，然後按一下 [確定]****。

4.  在 Active Directory Web 服務的 [屬性]**** 中，按一下 [登入]**** 索引標籤。

5.  選取 [本機系統帳戶]**** 選項，然後按一下 [確定]****。

### <a name="the-windows-update-service-should-use-the-local-system-account-as-its-logon-account"></a>Windows Update 服務應使用本機系統帳戶做為其登入帳戶
 **問題：** 自動更新服務的預設登入帳戶已變更。

 **影響：** 服務可能沒有如預期般運作所需的許可權。 如此一來，伺服器可能不會接收自動更新。

 **解決方案：**

##### <a name="to-change-the-windows-update-service-logon-account"></a>變更 Windows Update 服務登入帳戶

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [Windows Update]**** 服務，然後按一下 [屬性]。

3.  在 [登入]**** 索引標籤中選取 [本機系統帳戶]****。

### <a name="the-dhcp-client-service-should-use-the-nt-authoritylocalservice-account-as-its-logon-account"></a>DHCP 用戶端服務應該使用 NT AUTHORITY\LocalService 帳戶做為其登入帳戶
 **問題：** DHCP 用戶端服務的預設登入帳戶已變更。

 **影響：** 服務可能沒有如預期般運作所需的許可權。 如此一來，用戶端電腦不會收到來自伺服器的 IP 位址。

 **解決方案：**

##### <a name="to-change-the-dhcp-client-service-logon-account"></a>變更 DHCP 用戶端服務登入帳戶

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [DHCP 用戶端]**** 服務，然後按一下 [屬性]****。

3.  在 [登入]**** 索引標籤中，選取 [此帳戶]****，然後輸入** NT AUTHORITY\Local Service**。

### <a name="the-iis-admin-service-should-use-the-local-system-account-as-its-logon-account"></a>IIS Admin Service 應該使用本機系統帳戶做為其登入帳戶
 **問題：** IIS Admin 服務的預設登入帳戶已變更。

 **影響：** 服務可能沒有如預期般運作所需的許可權。 如此一來，您可能無法存取在伺服器上執行的網站，例如遠端 Web 存取。

 **解決方案：**

##### <a name="to-change-the-service-logon-account"></a>變更服務登入帳戶

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 IIS Admin Service]****，然後按一下 [屬性]****。

3.  在 [登入]**** 索引標籤中選取 [本機系統帳戶]****。

### <a name="the-world-wide-web-publishing-service-should-use-the-local-system-account-as-its-logon-account"></a>World Wide Web Publishing 服務應該使用本機系統帳戶做為其登入帳戶
 **問題：** World Wide Web 發行服務的預設登入帳戶已變更。

 **影響：** 服務可能沒有如預期般運作所需的許可權。 如此一來，您可能無法存取在伺服器上執行的網站，例如遠端 Web 存取。

 **解決方案：**

##### <a name="to-change-the-world-wide-web-publishing-service-logon-account"></a>變更 World Wide Web Publishing 服務登入帳戶

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [World Wide Web Publishing 服務]****，然後按一下 [屬性]****。

3.  在 [登入]**** 索引標籤中選取 [本機系統帳戶]****。

### <a name="the-remote-desktop-gateway-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>遠端桌面閘道服務應該使用 NT AUTHORITY\Network Service 帳戶做為其登入帳戶
 **問題：** 遠端桌面閘道服務的預設登入帳戶已變更。

 **影響：** 服務可能沒有適當的許可權可如預期般運作。 如此一來，使用者可能無法使用遠端 Web 存取來存取電腦。

 **解決方案：**

##### <a name="to-change-the-remote-desktop-gateway-service-logon-account"></a>變更遠端桌面閘道服務登入帳戶

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [遠端桌面閘道]**** 服務，然後按一下 [屬性]****。

3.  在 [登入]**** 索引標籤中，選取 [此帳戶]****，然後輸入 **NT AUTHORITY\Network Service**。

### <a name="the-windows-time-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Windows 時間服務應該使用 NT AUTHORITY\Network Service 帳戶做為其登入帳戶
 **問題：** Windows 時間服務的預設登入帳戶已變更。

 **影響：** 服務可能沒有適當的許可權可如預期般運作。 如此一來，可能會無法使用日期和時間同步處理。

 **解決方案：**

##### <a name="to-change-the-windows-time-service-logon-account"></a>變更 Windows 時間服務登入帳戶

1.  在伺服器上開啟 services.msc。

2.  以滑鼠右鍵按一下 [Windows 時間]**** 服務，然後按一下 [屬性]****。

3.  在 [登入]**** 索引標籤中，選取 [此帳戶]****，然後輸入** NT AUTHORITY\Local Service**。

### <a name="the-built-in-administrators-group-does-not-have-the-right-to-log-on-as-batch-job"></a>內建的系統管理員群組沒有權限以批次工作登入
 **問題：** 內建的 Administrators 群組不具有以批次工作登入的許可權。

 **影響：** 如果系統管理員建立警示，並將警示設定為在系統管理員未登入時執行，警示將會失敗，錯誤碼為2147943785。

 **解決方式：** 如需如何授與內建 Administrators 群組以批次工作登入許可權的相關資訊，請參閱[為內建的系統管理員群組授與以批次工作登入 (的權利](/previous-versions/orphan-topics/ws.11/jj635076(v=ws.11)) https://technet.microsoft.com/library/jj635076) 。

### <a name="the-windows-firewall-is-turned-off"></a>Windows 防火牆已關閉
 **問題：** Windows 防火牆已關閉。 預設值為啟用。

 **影響：** 根據您的防火牆設定，Windows 防火牆會封鎖某些資訊，使其無法通過伺服器，協助保護您的伺服器和網路免于惡意活動。

 **解決方案：**

##### <a name="to-turn-on-windows-firewall-on-the-server"></a>在伺服器上開啟 Windows 防火牆

1.  在伺服器上開啟控制台。

2.  在 [控制台] 中按一下 [系統及安全性]****，然後按一下 [Windows 防火牆]****。

3.  在 [Windows 防火牆] 按一下 [開啟或關閉 Windows 防火牆]****，選取 [開啟 Windows 防火牆]**** 選項，然後按一下 [確定]****。

### <a name="the-internal-network-adapter-is-not-configured-to-register-ip-address-in-dns"></a>內部網路介面卡未設定為在 DNS 中登錄 IP 位址
 **問題：** 內部網路介面卡未設定為在 DNS 中註冊其 IP 位址。

 **影響：** 如果內部網路介面卡的 IP 位址未在 DNS 中註冊，則可能無法使用伺服器的電腦名稱稱來存取伺服器。

 **解決方式：** 確認內部網路介面卡已設定為在 DNS 中註冊。

### <a name="dns-the-values-for-the-dns-forwardingtimeout-and-recursiontimeout-registry-key-are-identical"></a>DNS： DNS ForwardingTimeout 和 RecursionTimeout 登錄機碼的值完全相同
 **問題：** DNS ForwardingTimeout 登錄機碼的值不應該與 RecursionTimeout 登錄機碼的值相同。

 **影響：** 您可能無法依名稱存取網際網路資源。

 **解決方式：** 將 RecursionTimeout 登錄機碼的值設定為大於 ForwardingTimeout 機碼的值（位於 HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\DNS\Parameters. 的登錄中）。

### <a name="the-forward-dns-zone-for-your-active-directory-domain-does-not-allow-secure-updates"></a>Active Directory 網域的正向 DNS 區域不允許安全更新
 **問題：** 您應該將正向對應區域設定成隻允許安全的動態更新。

 **影響：** 當您啟用安全動態更新時，只有經過授權的使用者和主機可以對記錄進行變更。

 **解決方案：**

##### <a name="to-configure-the-forward-lookup-zone-for-your-active-directory-domain"></a>為您的 Active Directory 網域設定正向對應區域

1.  在伺服器上開啟 dnsmgmt.msc。

2.  以滑鼠右鍵按一下您 Active Directory 網域的正向對應區域，然後按一下 [屬性]****。

3.  在 [動態更新]**** 下拉式清單中選取 [只有安全的]****，然後按一下 [確定]****。

### <a name="the-forward-dns-zone-does-not-allow-secure-updates"></a>正向 DNS 區域不允許安全更新
 **問題：** 您應該設定 _msdcs. * 區域的正向對應區域，只允許安全的動態更新。

 **影響：** 當您啟用安全動態更新時，只有經過授權的使用者和主機可以對 msdcs. * 區域中的記錄進行變更。

 **解決方案：**

##### <a name="to-allow-secure-updates-in-the-_msdcs-zone"></a>允許 _msdcs 區域中的安全更新

1.  在伺服器上開啟 dnsmgmt.msc。

2.  以滑鼠右鍵按一下 _msdcs 區域的正向對應區域，然後按一下 [屬性]****。

3.  在 [動態更新]**** 下拉式清單中選取 [只有安全的]****，然後按一下 [確定]****。

### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>Internet Explorer 增強式安全性設定未啟用。
 **問題：** Internet Explorer 增強式安全性設定 (IE) 目前未啟用 Administrators 群組的功能。

 **影響：** 如果 Internet Explorer 增強式安全性設定未針對系統管理員群組啟用，您的伺服器和 Internet Explorer 會增加遭受惡意攻擊的風險，這可能會透過 Web 內容和應用程式腳本進行。

 **解決方案：**

##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>啟用 Internet Explorer 增強式安全性設定

1.  開啟伺服器上的 [伺服器管理員]**** ，然後按一下 [本機伺服器]****。

2.  在 [屬性]**** 窗格中，將 [IE 增強式安全性設定]**** 的設定變更為 [啟用]****，然後按一下 [確定]****。

### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>Internet Explorer 增強式安全性設定未啟用。
 **問題：** Internet Explorer 增強式安全性設定 (IE ESC) 目前未針對使用者群組啟用。

 **影響：** 如果 Internet Explorer 增強式安全性設定未針對使用者群組啟用，您的伺服器和 Internet Explorer 會增加惡意攻擊的風險，這可能會透過 Web 內容和應用程式腳本進行。

 **解決方案：**

##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>啟用 Internet Explorer 增強式安全性設定

1.  開啟 [伺服器管理員]****，然後按一下 [本機伺服器]****。

2.  在 [屬性]**** 窗格中，將 [IE 增強式安全性設定]**** 的設定變更為 [啟用]****，然後按一下 [確定]****。

### <a name="the-source-server-remains-in-active-directory-sites-and-services"></a>來源伺服器仍留在 Active Directory 網站及服務中
 **問題：** 執行 Windows Small Business Server 的來源伺服器仍然存在於 Active Directory 網站和服務的預設-第一個網站名稱中。

 **影響：** 如果來源伺服器仍留在 Active Directory 網站和服務中，用戶端電腦可能會遇到連線問題： s。

 **解決方式：** 您應該將來源伺服器降級，將它從網域中移除，然後從 Active Directory 的網站和服務，以及 Active Directory 的使用者和電腦刪除來源伺服器。

### <a name="source-server-remains-in-sbscomputer-ou"></a>來源伺服器仍留在 SBSComputer OU
 **問題：** 執行 Windows Small Business Server 的來源伺服器仍然存在於 Active Directory 的使用者和電腦中。

 **影響：** 如果來源伺服器仍留在 Active Directory 使用者和電腦中，用戶端電腦可能會遇到連線問題： s。

 **解決方式：** 您應該將來源伺服器降級，將它從網域中移除，然後從 Active Directory 的網站和服務，以及 Active Directory 的使用者和電腦刪除來源伺服器。

### <a name="a-group-policy-is-missing"></a>群組原則已遺失
 **問題：** 缺少預設網域原則群組原則。

 **影響：** 適當的網域功能必須要有預設網域原則。

 **解決方案：**

##### <a name="to-restore-a-missing-group-policy"></a>還原遺失的群組原則

1.  在伺服器上開啟 open gpmc.msc。

2.  在 [群組原則管理員] 中，展開網域樹系，並從主控台樹狀目錄搜尋 [預設網域原則]**** 群組原則物件。

3.  如果原則沒有出現在樹狀目錄中，請從系統狀態備份中還原。

### <a name="no-dns-name-server-resource-records"></a>沒有 DNS 名稱伺服器的資源記錄
 **問題：** 伺服器的正向對應區域中沒有 DNS 名稱伺服器 (NS) 資源記錄。

 **影響：** 如果 Active Directory 網域的正向對應區域中沒有 DNS 名稱伺服器 (NS) 資源記錄存在，使用者可能無法存取網路或網際網路上的資源。

 **解決方案：**

##### <a name="to-restore-missing-dns-name-server-resource-records"></a>還原遺失的 DNS 名稱伺服器資源記錄

1.  在伺服器上開啟 dnsmgmt.msc。

2.  在 [DNS 管理員] 中，以滑鼠右鍵按一下 Active Directory 網域的正向對應區域，然後按一下 [屬性]****。

3.  在 [名稱伺服器]**** 索引標籤上，確認設定是否正確。

4.  進行任何必要的變更，然後按一下 [確定]**** 以儲存設定。

### <a name="no-dns-name-server-records"></a>沒有 DNS 名稱伺服器記錄
 **問題：** 伺服器的 _msdcs 區域中沒有 DNS 名稱伺服器 (NS) 資源記錄 (例如： _msdcs。

 **影響：** 如果 Active Directory 網域的 _msdcs 區域中沒有 DNS 名稱伺服器 (NS) 資源記錄存在，使用者可能無法存取網路或網際網路上的資源。

 **解決方案：**

##### <a name="to-restore-missing-dns-name-server-records"></a>還原遺失的 DNS 名稱伺服器記錄

1.  在伺服器上開啟 dnsmgmt.msc。

2.  在 [DNS 管理員] 中，以滑鼠右鍵按一下 _msdcs 區域的正向對應區域，然後按一下 [屬性]****。

3.  在 [名稱伺服器]**** 索引標籤上，確認設定是否正確。

4.  進行任何必要的變更，然後按一下 [確定]**** 以儲存設定。

### <a name="no-dns-name-server-records"></a>沒有 DNS 名稱伺服器記錄
 **問題：** 委派的 _msdcs 正向對應區域中沒有 DNS 名稱伺服器 (NS) 資源記錄。

 **影響：** 如果委派的 _msdcs 正向對應區域中沒有 DNS 名稱伺服器 (NS) 資源記錄存在，DNS 伺服器服務就無法解析網域的 DNS 資源記錄，而且將無法啟動。

 **解決方案：**

##### <a name="to-reconfigure-missing-dns-name-server-records"></a>重新設定遺失的 DNS 名稱伺服器記錄

1.  在伺服器上開啟 dnsmgmt.msc。

2.  在 [DNS 管理員] 中，依序展開您的伺服器名稱及 [正向對應區域]****。

3.  按一下您 Active Directory 網域的正向對應區域 (例如：contoso.local)。

4.  委派的 _msdcs 區域顯示為灰色的資料夾。 以滑鼠右鍵按一下 _msdcs 區域，然後按一下 [屬性]****。

5.  在 [名稱伺服器]**** 索引標籤上，確認設定是否正確。

6.  進行任何必要的變更，然後按一下 [確定]**** 以儲存設定。

### <a name="authenticated-users-not-a-member-of-the-pre-windows-2000-compatible-access-group"></a>已驗證的使用者不是 Pre-Windows 2000 Compatible Access 群組的成員
 **問題：** 已驗證的使用者群組不是 Windows 前2000相容存取群組的成員。

 **影響：** 如果內建的已驗證使用者群組不是 Windows 前2000相容存取群組的成員，網路使用者可能會遇到「拒絕存取」錯誤。

 **解決方案：**

##### <a name="to-add-authenticated-users-to-the-pre-windows-2000-compatible-access-group"></a>將已驗證的使用者加入 Pre-Windows 2000 Compatible Access 群組

1.  在伺服器上開啟 dsa.msc。

2.  在 [Builtin]**** 資料夾中，以滑鼠右鍵按一下 [Pre-Windows 2000 Compatible Access]****，然後按一下 [屬性]****。

3.  按一下 [新增]****，輸入**已驗證的使用者**，然後按 [確定]**** 兩次。

### <a name="dns-client-not-configured"></a>DNS 用戶端未設定
 **問題：** DNS 用戶端未設定成僅指向伺服器的內部 IP 位址。

 **影響：** 如果 DNS 用戶端未設定成僅指向伺服器的內部 IP 位址，則 DNS 名稱解析可能會失敗。

 **解決方案：**

##### <a name="to-configure-dns-to-point-only-to-the-server-s-internal-ip-address"></a>將 DNS 設定為僅指向伺服器的內部 IP 位址

1.  從用戶端電腦開啟網路連線的 [屬性]**** 頁面。

2.  請確定 DNS 設定成僅指向伺服器的內部 IP 位址。

### <a name="default-application-pool-value-changed"></a>預設應用程式集區值已變更
 **問題：** DefaultAppPool 應用程式集區的工作者進程數目上限未設定為預設值1。

 **影響：** 使用者可能無法連接到 Windows Small Business Server web 服務。

 **解決方案：**

##### <a name="to-reset-maximum-worker-processes-for-the-default-application-pool"></a>重設預設應用程式集區的工作者處理序數上限

1.  在伺服器上開啟 Internet Information Services (IIS) 管理員。

2.  在 [IIS 管理員] 中，展開您的伺服器名稱，然後按一下 [應用程式集區]****。

3.  在 [應用程式集區]**** 中，以滑鼠右鍵按一下 [DefaultAppPool]****，然後按一下 [進階設定]****。

4.  在 [進階設定]**** 中，將 [工作者處理序數上限]**** 變更為 1，然後按一下 [確定]****。

5.  關閉 [進階設定]****，以滑鼠右鍵按一下 [DefaultAppPool]****，然後停止並重新啟動應用程式集區。

### <a name="application-pool-for-remote-web-access-does-not-use-the-default-account"></a>遠端 Web 存取的應用程式集區沒有使用預設帳戶
 **問題：** RemoteAppPool 應用程式集區未以預設帳戶執行。

 **影響：** 網路使用者可能無法存取遠端 Web 存取網站。

 **解決方案：**

##### <a name="to-reset-the-remote-application-pool-to-use-the-default-account"></a>將遠端應用程式集區重設為使用預設帳戶

1.  在伺服器上開啟 Internet Information Services (IIS) 管理員。

2.  在 [IIS 管理員] 中，展開您的伺服器名稱，然後按一下 [應用程式集區]****。

3.  在 [應用程式集區]**** 中，以滑鼠右鍵按一下 [RemoteAppPool]****，然後按一下 [進階設定]****。

4.  在 [進階設定]**** 中，將 [身分識別]**** 變更為 [NetworkService]****，然後按一下 [確定]****。

5.  關閉 [進階設定]****，以滑鼠右鍵按一下 [RemoteAppPool]****，然後停止並重新啟動應用程式集區。

### <a name="application-pool-for-remote-web-access-does-not-use-the-default-net-framework-version"></a>遠端 Web 存取的應用程式集區沒有使用預設 .NET Framework 版本
 **問題：** RemoteAppPool 應用程式集區不是以預設的 Microsoft .NET Framework 版本執行。

 **影響：** 網路使用者可能無法存取遠端 Web 存取網站。

 **解決方案：**

##### <a name="to-change-the-net-framework-version-used-by-the-remoteapppool"></a>變更 RemoteAppPool 所使用的 .NET Framework 版本

1.  在伺服器上開啟 Internet Information Services (IIS) 管理員。

2.  在 [IIS 管理員] 中，展開您的伺服器名稱，然後按一下 [應用程式集區]****。

3.  在 [應用程式集區]**** 中，以滑鼠右鍵按一下 [RemoteAppPool]****，然後按一下 [進階設定]****。

4.  在 [進階設定]**** 中，將 [.NET Framework 版本]**** 變更為 v4.0，然後按一下 [確定]****。

5.  關閉 [進階設定]****，以滑鼠右鍵按一下 [RemoteAppPool]****，然後停止並重新啟動應用程式集區。

### <a name="the-remoteaccesslog-file-is-larger-than-1-gb-in-size"></a>RemoteAccess.log 檔案的大小大於 1 GB
 **問題：** 如果 Remoteaccess 檔案的大小超過 1 GB，您可能會在系統磁片磁碟機上遇到磁碟空間不足的錯誤。

 **影響：** 如果 Remoteaccess 檔案太大，可能會造成可用空間問題： s （磁片磁碟機 c）

 **解決方式：** 備份伺服器之後，您可以刪除位於%ProgramData%\Microsoft\Windows Server\Logs\WebApps 資料夾中的 Remoteaccess .log 檔案。

### <a name="default-web-sites-log-directory-is-over-1-gb-in-size"></a>預設的網站記錄檔目錄大小超過 1 GB
 **問題：** 如果預設網站的記錄檔資料夾大小超過 1 GB，您可能會在系統磁片磁碟機上遇到磁碟空間不足的錯誤。

 **影響：** 如果預設網站的記錄檔資料夾太大，可能會造成可用空間問題： s，磁片磁碟機 C：

 **解決方式：** 在您備份伺服器，而且預設網站停止時，您可以刪除 C:\inetpub\logs\LogFiles\W3SVC1 資料夾中的記錄檔。 然後啟動預設網站。

### <a name="no-binding-for-ssl-on-all-ip-addresses"></a>所有 IP 位址上都沒有 SSL 的繫結
 **問題：** 在伺服器上的所有 IP 位址上，沒有安全通訊端層 (SSL) 的系結。

 **影響：** 如果 SSL 未系結至伺服器上的所有 IP 位址，則某些網站將無法供使用者使用。

 **解決方案：**

##### <a name="to-bind-ssl-to-all-ip-addresses-on-the-server"></a>將 SSL 繫結至伺服器上的所有 IP 位址

1.  在伺服器上開啟 Internet Information Services (IIS) 管理員。

2.  在 [IIS 管理員] 的 [連線]**** 窗格中，依序展開您的伺服器、[網站]****，以滑鼠右鍵按一下 [預設的網站]****，然後按一下 [編輯繫結]****。

3.  在 [網站繫結]**** 中，按一下 [新增]****，然後選取下列設定：

    -   **類型**  = **HTTPs**

    -   **IP 位址**  = **全部未指派**

    -   [連接埠]**** = [443]

4.  選取 SSL 憑證，然後按一下 [確定]**** 儲存變更。

### <a name="no-binding-for-ssl-on-the-default-web-site"></a>預設網站上沒有 SSL 的繫結
 **問題：** 預設網站上沒有 SSL 的系結。

 **影響：** 如果 SSL 未系結至預設網站，則某些網站可能無法供使用者使用。

 **解決方案：**

##### <a name="to-bind-ssl-to-the-default-website"></a>將 SSL 繫結至預設網站

1.  在伺服器上開啟 Internet Information Services (IIS) 管理員。

2.  在 [IIS 管理員] 的 [連線]**** 窗格中，依序展開您的伺服器、[網站]****，以滑鼠右鍵按一下 [預設的網站]****，然後按一下 [編輯繫結]****。

3.  在 [網站繫結]**** 中按一下 [新增]****，然後選取下列選項：

    -   **類型**  = **HTTPs**

    -   **IP 位址**  = **全部未指派**

    -   [連接埠]**** = [443]

    > [!NOTE]
    >  如果連接埠 443 的 HTTPS 繫結有特定的 IP 位址，將該繫結的 [IP 位址]**** 屬性變更為 [全部解除指派]****。 IP 位址 127.0.0.1 則是例外狀況。 請不要變更 127.0.0.1 的繫結。

4.  選取 SSL 憑證，然後按一下 [確定]**** 儲存變更。

### <a name="a-certificate-expires-within-30-days"></a>憑證在 30 天內過期
 **問題：** 您的伺服器憑證將在30天內過期。

 **影響：** 伺服器無法使用過期的憑證。 如果憑證過期，使用者可能無法使用隨處存取函數。

 **解決方式：** 若要防止憑證過期，請使用您信任的憑證授權單位單位來更新憑證。

### <a name="certificate-subject-does-not-match-the-name-configured-by-domain-name-wizard"></a>憑證主體與網域名稱精靈所設定的名稱不相符
 **問題：** 憑證主體與功能變數名稱 wizard 所設定的名稱不相符。

 **影響：** 如果憑證主體與 [功能變數名稱] [wizard] 所設定的名稱不相符，部分網站將不會初始化。 其他網站會顯示「此網站的安全性憑證有問題」錯誤。

 **解決方式：** 若要解決此問題：，請再次執行 [設定隨處存取嚮導]，並提供正確的憑證功能變數名稱，或購買符合您想要使用之功能變數名稱的新憑證。

### <a name="one-or-more-user-accounts-have-duplicate-cn-names"></a>一或多個使用者帳戶有重複的 CN 名稱
 **問題：** 一或多個使用者帳戶有重複的 CN 名稱： {0} 。

 **影響：** 如果使用者帳戶有重複的 CN 名稱，使用者可能無法登入網路。 此外，搜尋 Active Directory 的使用者會傳回不正確的值。

 **解決方式：** 若要解決此問題：，請確定網路使用者帳戶沒有重複的 "CN =" 名稱。 若要讓您事半功倍，請考慮將 Active Directory 內容匯出到文字檔供檢閱。 如需如何執行這項操作的詳細資訊，請參閱[使用 LDIFDE 將目錄物件匯入和匯出至 Active Directory (知識庫文章 237677) ](https://support.microsoft.com/kb/237677) (https://support.microsoft.com/kb/237677) 。

### <a name="nt-backup-is-installed"></a>NT Backup 已安裝
 **問題：** Windows NT Backup 程式會安裝在伺服器上。

 **影響：**  Windows Server Essentials 使用 Windows Server Backup。 如果同時安裝了 Windows NT Backup 程式，這兩個備份程式之間會產生衝突。 這可能導致 Windows Server Backup 程序失敗。 衝突也可能會讓您無法使用備份來還原伺服器。

 **解決方式：** 若要解決此問題：，請從伺服器卸載 NT 備份程式。

### <a name="iis-does-not-own-port-80-000080-or-port-443-0000443"></a>IIS 並未擁有連接埠 80 (0.0.0.0:80) 或連接埠 443 (0.0.0.0: 443)
 **問題：** Internet Information Services (IIS) 不會擁有埠 80 (0.0.0.0： 80) 或埠443。 這些連接埠目前是由其他應用程式繫結。

 **影響：**  Windows Server Essentials web 應用程式需要使用埠80和埠443，才能將服務提供給使用者。 如果其他進程或應用程式已經在使用埠80或埠443，則無法執行 Windows Server Essentials web 應用程式。 如果發生這種情況，則遠端 Web 存取和其他應用程式無法供使用者使用。

 **解決方式：** 若要解決此問題：，請將已在使用埠80或埠443的應用程式卸載，或將該應用程式指派給不同的埠。

### <a name="the-default-website-is-not-running"></a>預設的網站未執行
 **問題：** 預設網站未在您的 Windows Server Essentials 環境中執行。

 **影響：**  Windows Server Essentials web 應用程式需要使用預設的網站。 如果預設的網站未執行，則遠端 Web 存取和其他應用程式無法供使用者使用。

 **解決方案：**

##### <a name="to-start-the-default-website"></a>啟動預設的網站

1.  在伺服器上開啟 Internet Information Services (IIS) 管理員。

2.  在 [IIS 管理員] 中，展開您的伺服器名稱，然後按一下 [網站]****。

3.  以滑鼠右鍵按一下 [預設的網站]****，指向 [管理網站]****，然後按一下 [啟動]****。

### <a name="read-and-script-permissions-for-the-remote-virtual-directory-are-incorrect"></a>/Remote 虛擬目錄的讀取和指令碼權限不正確
 **問題：** 未將讀取和腳本許可權指派給/Remote 虛擬目錄。

 **影響：** 如果/Remote 虛擬目錄的讀取和腳本許可權不正確，使用者就無法使用遠端 Web 存取。 當他們嘗試使用遠端 Web 存取來流覽網際網路時，可能會遇到「HTTP 錯誤403.1 禁止」錯誤。

 **解決方案：**

##### <a name="to-assign-read-and-script-permissions-to-the-remote-directory"></a>將讀取和指令碼權限指派給 /Remote 目錄

1.  在伺服器上開啟 Internet Information Services (IIS) 管理員。

2.  在 [IIS 管理員] 中，展開您的伺服器名稱，然後按一下 [網站]****。

3.  展開 [預設的網站]****，然後展開 [遠端]****。

4.  在 [功能檢視]**** 中，按兩下 [處理常式對應]****。

5.  在 [動作]**** 窗格中按一下 [編輯功能權限]****。

6.  選取 [讀取]**** 和 [指令碼]**** 核取方塊，然後按一下 [確定]****。

### <a name="http-redirect-is-either-set-or-inherited-on-the-remote-virtual-directory"></a>/Remote 虛擬目錄上設定或繼承了 HTTP 重新導向
 **問題：** 在/Remote 虛擬目錄上，未預期地設定或繼承 HTTP 重新導向屬性。

 **影響：** 如果在/Remote 虛擬目錄上設定 HTTP 重新導向屬性，遠端 Web Workplace 將無法正常運作。

 **解決方案：**

##### <a name="to-remove-the-http-redirect-attribute"></a>移除 HTTP 重新導向屬性

1.  在伺服器上開啟 Internet Information Services (IIS) 管理員。

2.  在 [IIS 管理員] 中，展開您的伺服器名稱，然後按一下 [網站]****。

3.  展開 [預設的網站]****，然後展開 [遠端]****。

4.  在 [功能檢視]**** 中，按兩下 [HTTP 重新導向]****。

5.  清除 [將要求重新導向至此目的地]**** 核取方塊，然後按一下 [動作]**** 窗格上的 [套用]****。

### <a name="a-host-name-exists-for-port-80-on-the-default-website"></a>預設網站上的連接埠 80 有一個主機名稱。
 **問題：** 預設網站上的埠80會指派主機名稱。

 **影響：** 如果在預設網站上將主機名稱指派給埠80，您可能無法連接到某些 Windows Server Essentials web 應用程式。 在此情況下並不需要主機名稱，也不建議使用。

 **解決方案：**

##### <a name="to-clear-the-host-name-entry-for-port-80-on-the-default-website"></a>清除預設網站的連接埠 80 上的主機名稱項目

1.  在伺服器上開啟 Internet Information Services (IIS) 管理員。

2.  在 [IIS 管理員] 中，展開您的伺服器名稱，然後按一下 [網站]****。

3.  在 [功能檢視]**** 中，以滑鼠右鍵按一下 [預設的網站]****，然後按一下 [繫結]****。

4.  在 [網站繫結]**** 中選取 [通訊埠 80 的 http]**** 設定，然後按一下 [編輯]****。

5.  在 [編輯網站繫結]**** 中、清除 [主機名稱]**** 項目，然後按一下 [確定]****。

### <a name="backup-does-not-succeed-because-of-a-hidden-partition"></a>備份因為隱藏式磁碟分割而沒有成功
 **問題：** 非 NTFS 磁碟分割已排程 Windows Server Backup 的備份。

 **影響：** Windows Server Backup 只能備份格式化為 NTFS 的磁碟分割。

 **解決方式：** 請勿設定 Windows Server Backup 來備份非 NTFS 磁碟分割。 如需詳細資訊，請參閱[當 Windows Server 2008 型電腦上的系統狀態備份失敗時，會記錄事件識別碼12290和 16387 (知識庫文章 968128) ](https://support.microsoft.com/kb/968128) (https://support.microsoft.com/kb/968128) 。

### <a name="the-most-recent-backup-did-not-succeed"></a>最新的備份工作未成功
 **問題：** 最近的備份嘗試未順利完成。

 **影響：** 系統的備份狀態不正確。

 **解決方式：** 檢查事件記錄檔和備份記錄檔，以瞭解最近一次備份期間所發生的錯誤。

### <a name="the-startup-type-for-the-file-replication-service-is-not-set-to-automatic"></a>檔案複寫服務的啟動類型未設定為自動
 **問題：** 如果啟動類型未設定為預設值 [自動]，則 (FRS) 的檔案複寫服務可能不會啟動。

 **影響：** 如果檔案複寫服務不在執行中，網域控制站可能會停止通告其服務。 這可能會導致其他問題，例如登入錯誤和群組原則錯誤。

 **解決方案：**

##### <a name="to-configure-the-file-replication-service-for-automatic-startup"></a>將檔案複寫服務設定為自動啟動

1.  開啟 [服務] 主控台。

2.  在服務清單中按兩下 [檔案複寫]****。

3.  針對 [啟動類型]**** 選取 [自動]****，然後按一下 [套用]****。

### <a name="the-file-replication-service-is-not-running"></a>檔案複寫服務未執行
 **問題：** 檔案複寫服務不在執行中。

 **影響：** 如果檔案複寫服務不在執行中，網域控制站可能會停止通告其服務。 這種行為可能會導致其他問題，例如登入錯誤和群組原則錯誤。

 **解決方案：**

##### <a name="to-start-the-file-replication-service"></a>啟動檔案複寫服務

1.  開啟 [服務] 主控台。

2.  在服務清單中按兩下 [檔案複寫服務]****。

3.  按一下 [啟動]。

### <a name="the-logon-account-for-the-file-replication-service-is-not-set-to-use-the-local-system-account"></a>檔案複寫服務的登入帳戶未設定為使用本機系統帳戶
 **問題：** 檔案複寫服務未設定為使用本機系統帳戶做為預設登入帳戶。

 **影響：** 如果檔案複寫服務未使用本機系統做為預設登入帳戶，您可能會遇到許可權相關的錯誤。 這些錯誤會觸發其他錯誤，並且最終可能造成網域控制站停止通告其服務。

 **解決方案：**

##### <a name="to-configure-local-system-as-the-default-logon-account-for-file-replication"></a>將本機系統設為檔案複寫的預設登入帳戶

1.  開啟 [服務] 主控台。

2.  在服務清單中按兩下 [檔案複寫]****。

3.  在 **[服務內容]** 頁面上，按一下 [登入]**** 索引標籤。

4.  選取 [本機系統帳戶]**** 選項，然後按一下 [套用]****。

5.  重新啟動服務。

### <a name="the-startup-type-for-the-dfs-replication-service-is-not-set-to-automatic"></a>DFS 複寫服務的啟動類型未設定為自動
 **問題：** 如果啟動類型未設定為預設值 [自動]，則 DFS 複寫服務可能不會啟動。

 **影響：** 如果 DFS 複寫服務並未執行，網域控制站可能會停止通告其服務。 這可能會導致其他問題，例如登入錯誤和群組原則錯誤。

 **解決方案：**

##### <a name="to-configure-the-dfs-replication-service-for-automatic-startup"></a>將 DFS 複寫服務設定為自動啟動

1.  開啟 [服務] 主控台。

2.  在服務清單中按兩下 [DFS 複寫]****。

3.  針對 [啟動類型]**** 選取 [自動]****，然後按一下 [套用]****。

### <a name="the-dfs-replication-service-is-not-running"></a>DFS 複寫服務未執行
 **問題：** DFS 複寫服務目前不在執行中。

 **影響：** 如果 DFS 複寫服務並未執行，網域控制站可能會停止通告其服務。 這種行為可能會導致其他問題，例如登入錯誤和群組原則錯誤。

 **解決方案：**

##### <a name="to-start-the-dfs-replication-service"></a>啟動 DFS 複寫服務

1.  開啟 [服務] 主控台。

2.  在服務清單中按兩下 [DFS 複寫]****。

3.  按一下 [啟動]。

### <a name="the-dfs-replication-service-is-not-is-not-set-to-use-the-local-system-account"></a>DFS 複寫服務未設定為使用本機系統帳戶
 **問題：** DFS 複寫服務未設定為使用本機系統帳戶做為預設登入帳戶。

 **影響：** 如果 DFS 複寫服務未使用本機系統做為預設登入帳戶，您可能會遇到許可權相關的錯誤。 這些錯誤會觸發其他錯誤，並且最終可能造成網域控制站停止通告其服務。

 **解決方案：**

##### <a name="to-configure-dfs-replication-to-use-local-system-as-the-default-logon-account"></a>將 DFS 複寫設定為使用本機系統做為預設登入帳戶

1.  開啟 [服務] 主控台。

2.  在服務清單中按兩下 [DFS 複寫]****。

3.  在 **[服務內容]** 頁面上，按一下 [登入]**** 索引標籤。

4.  選取 [本機系統帳戶]**** 選項，然後按一下 [套用]****。

5.  重新啟動服務。

### <a name="the-windows-server-office-365-integration-service-is-not-set-to-use-the-local-system-account"></a>Windows Server Office 365 整合服務未設定為使用本機系統帳戶
 **問題：** Windows Server Office 365 整合服務未設定為使用本機系統帳戶做為預設登入帳戶。

 **影響：** 如果 Windows Server Office 365 整合服務未使用本機系統做為預設登入帳戶，Office 365 的某些功能可能無法正常運作。 您也可能會遇到權限相關的錯誤。

 **解決方案：**

##### <a name="to-configure-the-office-365-integration-service-to-use-local-system-as-the-default-logon-account"></a>將 Office 365 整合服務設定為使用本機系統做為預設登入帳戶

1.  開啟 [服務] 主控台。

2.  在服務清單中按兩下 [Windows Server Office 365 整合服務]****。

3.  在 **[服務內容]** 頁面上，按一下 [登入]**** 索引標籤。

4.  選取 [本機系統帳戶]**** 選項，然後按一下 [套用]****。

5.  重新啟動服務。

### <a name="the-windows-server-office-365-integration-service-is-not-running"></a>Windows Server Office 365 整合服務未執行
 **問題：** Windows Server Office 365 整合服務目前不在執行中。

 **影響：** 如果 Windows Server Office 365 整合服務並未執行，則無法使用 Office 365 的雲端式功能。

 **解決方案：**

##### <a name="to-start-the-windows-server-office-365-integration-service"></a>啟動 Windows Server Office 365 整合服務

1.  開啟 [服務] 主控台。

2.  在服務清單中按兩下 [Windows Server Office 365 整合服務]****。

3.  按一下 [啟動]。

### <a name="the-startup-type-for-the-windows-server-office-365-integration-service-is-not-set-to-automatic"></a>Windows Server Office 365 整合服務的啟動類型未設定為自動
 **問題：** 如果啟動類型未設定為預設值 [自動]，則 Windows Server Office 365 整合服務可能不會啟動。

 **影響：** 如果 Windows Server Office 365 整合服務並未執行，則無法使用 Office 365 的雲端式功能。

 **解決方案：**

##### <a name="to-configure-the-office-365-integration-service-for-automatic-startup"></a>將 Office 365 整合服務設定為自動啟動

1.  開啟 [服務] 主控台。

2.  在服務清單中按兩下 [Windows Server Office 365 整合服務]****。

3.  針對 [啟動類型]**** 選取 [自動]****，然後按一下 [套用]****。

### <a name="a-registry-value-is-missing-or-set-incorrectly"></a>登錄值遺失或設定不正確
 **問題：** HKEY_LOCAL_MACHINE \Software\Microsoft\Rpc\RpcProxy 下的登錄機碼可能包含不正確的值，或不存在。

 **影響：** 如果 RPCProxy 登錄機碼設定不正確，您可能會收到類似下列的錯誤訊息：「您的電腦無法連線到遠端電腦，因為遠端桌面閘道伺服器暫時無法使用。 請稍後再重新連線或連絡網路系統管理員尋求協助。」

 **解決方案：**

##### <a name="to-correct-the-registry-setting"></a>修正登錄設定

1.  開啟 [登錄編輯程式]。

2.  瀏覽到以下的登錄機碼：

     HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy

3.  請確定名為 "site" 的字串具有 [預設網站] 的資料值：

    -   如果資料值不正確，請修改字串以使用正確的值。

    -   如果該字串不存在，請建立名為 "site" 的新字串，並將資料值設定為 [預設的網站]。

### <a name="the-startup-type-for-the-block-level-backup-engine-service-is-not-set-to-manual"></a>區塊層級備份引擎服務的啟動類型未設定為手動
 **問題：** 區塊層級備份引擎服務未使用預設的手動啟動類型。

 **影響：** 如果 [啟動類型] 未設定為 [手動]，區塊層級備份引擎服務可能不會啟動。 此問題：可能會導致 Windows Server Backup 作業失敗。

 **解決方案：**

##### <a name="to-configure-the-block-level-backup-engine-service-for-manual-startup"></a>將區塊層級備份引擎服務設定為手動啟動

1.  開啟 [服務] 主控台。

2.  在服務清單中按兩下 [區塊層級備份引擎服務]****。

3.  針對 [啟動類型]**** 選取 [手動]****，然後按一下 [套用]****。

### <a name="the-logon-account-for-the-block-level-backup-engine-service-is-not-set-to-use-the-local-system-account"></a>區塊層級備份引擎服務的登入帳戶未設定為使用本機系統帳戶
 **問題：** 區塊層級備份引擎服務未設定為使用本機系統帳戶做為預設登入帳戶。

 **影響：** 如果區塊層級備份引擎服務未使用本機系統做為預設登入帳戶，您可能會遇到許可權相關的錯誤。 這些錯誤會造成 Windows Server Backup 工作無法順利完成。

 **解決方案：**

##### <a name="to-configure-the-block-level-backup-engine-service-to-use-local-system-as-the-default-logon-account"></a>將區塊層級備份引擎服務設定為使用本機系統做為預設登入帳戶

1.  開啟 [服務] 主控台。

2.  在服務清單中按兩下 [區塊層級備份引擎服務]****。

3.  在 **[服務內容]** 頁面上，按一下 [登入]**** 索引標籤。

4.  選取 [本機系統帳戶]**** 選項，然後按一下 [套用]****。

5.  重新啟動服務。

### <a name="the-common-name-on-the-certificate-that-is-bound-to-the-wss-certificate-web-service-website-does-not-match-the-server-name"></a>與 WSS 憑證 Web 服務網站繫結的憑證一般名稱與伺服器名稱不相符
 **問題：** 不有效的憑證會系結至 IIS 中的 WSS 憑證 Web 服務網站。 此憑證的一般名稱與伺服器名稱不相符。

 **影響：** 如果您將不正確憑證系結至 WSS 憑證 Web 服務網站，則 Connect Wizard 可能無法正常運作。

 **解決方案：**

##### <a name="to-configure-a-valid-certificate-for-the-wss-certificate-web-service"></a>為 WSS 憑證 Web 服務設定有效的憑證

1.  在伺服器上開啟 Internet Information Services (IIS) 管理員。

2.  在 [IIS 管理員] 中，展開您的伺服器名稱，然後按一下 [網站]****。

3.  以滑鼠右鍵按一下 [WSS 憑證 Web 服務]****，然後按一下 [編輯繫結]****。

4.  在 **[網站繫結]** 中，按一下 [HTTPS]****，然後按一下 [編輯]****。

5.  在 **[編輯網站繫結]** 的 **[SSL 憑證]** 中，選取與您伺服器名稱相同的憑證。

6.  如果有一個以上的憑證項目與您伺服器的名稱相同，按一下 [檢視]**** 判斷哪一個憑證有效，然後選取適當的憑證。

### <a name="there-appears-to-be-a-problem-with-the-certificate-binding-for-the-remote-desktop-gateway-service"></a>遠端桌面閘道服務的憑證繫結似乎有問題
 **問題：** 遠端桌面閘道服務的憑證似乎系結錯誤。

 **影響：** 如果未正確設定遠端桌面閘道服務的憑證，使用者將無法連線到遠端 Web 存取。

 **解決方案：**

##### <a name="to-fix-the-binding-for-the-remote-desktop-gateway-service"></a>修正遠端桌面閘道服務的繫結

-   以系統管理員身分開啟命令提示字元，並輸入下列命令：

    ```
    REG ADD HKLM\SYSTEM\CurrentControlSet\services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443  /v DefaultFlags /t REG_DWORD /d 1 /f
    net stop tsgateway
    net start tsgateway
    ```

     如需詳細資訊，請參閱[如何在 Windows Server Essentials 中管理遠端桌面閘道服務 (知識庫文章 2472211) ](https://support.microsoft.com/kb/2472211) (https://support.microsoft.com/kb/2472211) 。