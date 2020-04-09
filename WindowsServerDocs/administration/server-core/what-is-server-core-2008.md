---
title: 什麼是 Server Core 2008？
description: 瞭解 Windows Server 2008 中的 Server Core 安裝選項
ms.prod: windows-server
ms.author: helohr
ms.date: 11/01/2017
ms.topic: article
author: heidilohr
ms.openlocfilehash: 13167dad848ac31827c42045360e45c76718207a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851591"
---
# <a name="what-is-server-core-2008"></a>什麼是 Server Core 2008？
>適用于： Windows Server 2008

>[!NOTE]
>這套資訊適用于 Windows Server 2008。 如需 Windows Server 中 Server Core 的相關資訊，請參閱[什麼是 Windows server 中的 Server Core 安裝](https://docs.microsoft.com/windows-server/administration/server-core/what-is-server-core)。 

[Server Core] 選項是新的最小安裝選項，可在您部署 Standard、Enterprise 或 Datacenter edition 的 Windows Server 2008 時使用。 Server Core 提供最低限度的 Windows Server 2008 安裝，只支援安裝特定的伺服器角色，如本章稍後所述。 這與 Windows Server 2008 的完整安裝選項相反，它支援安裝所有可用的伺服器角色，以及其他 Microsoft 或協力廠商伺服器應用程式，例如 Microsoft Exchange Server 或 SAP。 

在繼續進行之前，必須先說明「安裝選項」的片語。 一般來說，當您購買 Windows Server 2008 的複本時，您會購買授權以使用特定版本或庫存單位（Sku）。 表1-1 列出可用的各種 Windows Server 2008 版本。 此表也會指出每個版本都可以使用哪些安裝選項（完整、伺服器核心或兩者）。

**資料表 1-1**Windows Server 2008 版本和其安裝選項的支援

| 版本       | 完整          | Server Core  |
| ------------- | :-------------: | :------------: |
| Windows Server 2008 Standard （x86 和 x64）       | X | X        |
| Windows Server 2008 Enterprise （x86 和 x64）       | X | X        |
| Windows Server 2008 Datacenter （x86 和 x64）        | X | X       |
| Windows Web 服務器2008（x86 和 x64）       | X | X  |
| 適用于 Itanium 型系統的 Windows Server 2008       | X |     |
| Windows HPC Server 2008 （僅限 x64）       | X |   |
| Windows Server 2008 Standard （不含 Hyper-v）（x86 和 x64） | X | X |
| Windows Server 2008 Enterprise （不含 Hyper-v）（x86 和 x64）  | X | X |
| Windows Server 2008 Standard （不含 Hyper-v）（x86 和 x64） | X | X |

若要瞭解什麼是「安裝選項」，讓我們假設您已購買大量授權，可讓您安裝 Windows Server 2008 Enterprise Edition 的複本。 當您將大量授權的媒體插入系統並開始安裝程式時，您會看到的其中一個畫面（如圖1-1 所示）會提供您選擇的版本和安裝選項。

![選取要安裝的 Server Core 安裝選項](../media/what-is-server-core-2008/FIg1-1.png)

**圖 1-1**選取要安裝的 Server Core 安裝選項

在圖1-1 中，您的大量授權（或零售媒體的產品金鑰）會提供您兩個安裝選項：第二個選項（Windows Server 2008 Enterprise 的完整安裝）和第五個選項（Windows Server 2008 Enterprise 的 Server Core 安裝），並在此範例中選取後者。 

## <a name="full-vs-server-core"></a>Full 與 Server Core 的比較 
從 Microsoft Windows 平臺的初期開始，Windows 伺服器基本上是「一切」伺服器，其中包含所有種類的功能，有些則可能永遠不會在網路環境中使用。 例如，當您在系統上安裝 Windows Server 2003 時，即使您不需要此服務，也會在伺服器上安裝路由及遠端存取服務（RRAS）的二進位檔（雖然您仍然必須設定並啟用 RRAS 才能正常執行）。 Windows Server 2008 只有在您選擇在伺服器上安裝該特定角色時，才會藉由安裝伺服器角色所需的二進位檔來改善舊版。 不過，Windows Server 2008 的完整安裝選項仍然會安裝許多服務，以及特定使用案例通常不需要的其他元件。 

這就是 Microsoft 為 Windows Server 2008 建立第二個安裝選項（Server Core）的原因：若要排除對某些常用伺服器角色的支援不重要的任何服務和其他功能。 例如，網域名稱系統（DNS）伺服器不需要安裝 Windows Internet Explorer，因為基於安全考慮，您不會想要從 DNS 伺服器流覽網路。 而且 DNS 伺服器甚至不需要圖形化使用者介面（GUI），因為您可以從命令列使用強大的 Dnscmd 命令，或從遠端使用 DNS Microsoft Management Console （MMC）嵌入式管理單元來管理 DNS 的所有層面。

為了避免這種情況，Microsoft 決定從 Windows Server 2008 中去除所有內容，而這對於執行核心網路服務（例如 Active Directory Domain Services （AD DS）、DNS、動態主機設定通訊協定（DHCP）、檔案和列印，以及一些其他伺服器角色而言並不是絕對必要。 結果是新的 Server Core 安裝選項，可用來建立僅支援有限數目角色和功能的伺服器。 

## <a name="the-server-core-gui"></a>Server Core GUI
當您完成在系統上安裝 Server Core 並第一次登入時，您會感到驚訝。 圖1-2 顯示第一次登入後的 Server Core 使用者介面。

![Server Core 使用者介面](../media/what-is-server-core-2008/Fig1-2.png)

**圖 1-2**Server Core 使用者介面

沒有桌面！ 也就是說，沒有 Windows Explorer shell，其 [開始] 功能表、工作列，以及您可能會用到的其他功能。 您只需要一個命令提示字元，這表示您必須一次輸入一個命令（速度較慢），或使用腳本和批次檔，來完成設定 Server Core 安裝的大部分工作，這可協助您藉由將它們自動化來加速和簡化您的設定工作。 當您執行 Server Core 的自動安裝時，您也可以使用回應檔案來執行一些初始設定工作。 

對於使用 Netsh、Dfscmd 和 Dnscmd 等命令列工具的系統管理員而言，設定及管理 Server Core 安裝可能很簡單，甚至更有趣。 不過，對於不是專家的人來說，全都不會遺失。 您仍然可以使用標準的 Windows Server 2008 MMC 工具來管理 Server Core 安裝。 您只需要在執行 Windows Server 2008 或 Windows Vista Service Pack 1 完整安裝的不同系統上使用它們。 

您會在本書的第3到6章中深入瞭解如何設定及管理 Server Core 安裝，而後續章節則討論如何管理特定的伺服器角色和其他元件。 若要深入瞭解各種 Windows 命令列工具，以及如何使用它們，有兩個有用的資源可供您參考：
* Windows Server 2008 技術文件庫（）的命令參考區段 
* *Windows 命令列管理員的 Pocket 顧問*，由 William Stanek （Microsoft 按下，2008） 

[表 1-2] 列出主要 GUI 應用程式及其可執行檔，其可在 Server Core 安裝中使用。

**資料表 1-2**Server Core 安裝中提供的 GUI 應用程式

| GUI 應用程式 | 具有路徑的可執行檔 |
| -------------   | -------------       | 
| 命令提示字元 | %WINDIR%\System32\Cmd.exe |
| Microsoft 支援服務診斷工具 | %WINDIR%\System32\MSdt.exe |
| 記事本 | %WINDIR%\System32\Notepad.exe |
| 登錄編輯程式 | %WINDIR%\System32\Regedt32.exe |
| 系統資訊 | %WINDIR%\System32\MSinfo32.exe |
| 工作管理員 | %WINDIR%\System32\Taskmgr.exe |
| Windows Installer | %WINDIR%\System32\MSiexec.exe |

這是非常簡短的清單！ 以下是不包含在 Server Core 中的使用者介面元素清單：
* Windows Explorer desktop shell （Explorer）和任何支援的功能，例如主題 
* 所有 MMC 主控台 
* 所有的控制台公用程式，但地區和語言選項（國際 cpl）和日期和時間（Timedate 時間）除外 
* 所有超文字標記語言 (HTML) （HTML）轉譯引擎，包括 Internet Explorer 和 HTML 說明 
* Windows Mail 
* Windows Media Player 
* 大部分的配件，例如繪製、計算機和 Wordpad

伺服器核心中也不會顯示 .NET Framework，這表示不支援在 Server Core 安裝上執行 managed 程式碼。 只有機器碼（使用 Windows 應用程式開發介面（Api）撰寫的程式碼）可以在 Server Core 上執行。 總而言之，相依于 .NET Framework 或在 node.js shell 上的任何 GUI 應用程式，都不會在 Server Core 上執行。 

>[!NOTE]
>由於 Windows PowerShell 需要 .NET Framework，因此您無法將 Windows PowerShell 安裝到 Server Core。 不過，只要您只使用 PowerShell WMI 命令，就可以使用 Windows PowerShell 從遠端系統管理 Server Core 安裝。

## <a name="supported-server-roles"></a>支援的伺服器角色 
相較于 Windows Server 2008 的完整安裝，Server Core 安裝只包含有限數量的伺服器角色。 表1-3 比較適用于 Windows Server 2008 Enterprise Edition 的完整和 Server Core 安裝的角色。 

**資料表 1-3**Windows Server 2008 Enterprise Edition 的完整和 Server Core 安裝的伺服器角色比較

| 伺服器角色  | 可在完整安裝中使用  | 在 Server Core 中提供  |
| ------------- | :-------------: | :------------: |
| Active Directory 憑證服務 (AD CS)  | X |  |
| Active Directory 網域服務 (AD DS)  | X  | X |
| Active Directory Federation Services (AD FS)  | X  |  |
| Active Directory 輕量型目錄服務 (AD LDS)  | X  | X |
| Active Directory Rights Management Services (AD RMS)  | X  |  |
| 應用程式伺服器  | X  |  |
| DHCP 伺服器  | X  | X |
| DNS 伺服器  | X  | X |
| 傳真伺服器  | X  |  |
| 檔案服務  | X  | X |
| Hyper-V  | X | X |
| 網路原則與存取服務  | X  |  |
| 列印服務  | X  | X |
| 串流處理媒體服務  | X  | X |
| 終端機服務  | X  |  |
| UDDI 服務  | X  |  |
| Web 伺服器 (IIS) | X | X |
| Windows 部署服務  | X |  |

雖然無論架構（x86 或 x64）和產品版本為何，適用于 Server Core 的角色通常都相同，但有一些例外狀況：
* 只有在使用 Hyper-v 產品媒體購買 Windows Server 2008 （Hyper-v 僅適用于 x64 版本）時，才能使用 Hyper-v （虛擬化）角色。 如果您不需要此角色，您可以改為購買 Windows Server 2008，但不含 Hyper-v 產品媒體。 
* Standard Edition 上的檔案服務角色受限於一個獨立分散式檔案系統（DFS）根目錄，且不支援跨檔案複寫（DFS-R）。 
* 您必須先從 Microsoft 下載中心下載並安裝伺服器架構（x86 或 x64）的適當 Microsoft Update 獨立封裝（.msu 檔案），才能在 Server Core 上安裝串流處理媒體服務角色。
* 網頁伺服器（IIS）角色不支援 ASP.NET。 這是因為 Server Core 上不支援 .NET Framework，這會限制您可以使用 Server Core Web 服務器執行的工作。 

## <a name="supported-optional-features"></a>支援的選擇性功能
Server Core 安裝也只支援 Windows Server 2008 完整安裝上可用的有限功能子集。 表1-4 比較 Windows Server 2008 Enterprise Edition 的完整和 Server Core 安裝可用的功能。

**資料表 1-4**Windows Server 2008 Enterprise Edition 完整和 Server Core 安裝的功能比較

| 功能  | 可在完整安裝中使用  | 在 Server Core 中提供  |
| ------------- | :-------------: | :------------: |
| .NET Framework 3.0 功能  | X  |  |
| BitLocker 磁碟機加密  | X  | X |
| BITS 伺服器擴充功能  | X  |  |
| 連線管理員系統管理組件  | X |  |
| 桌面體驗  | X |  |
| 容錯移轉叢集  | X  | X |
| 群組原則管理  | X  |  |
| 網際網路列印用戶端  | X  |  |
| Internet Storage Name Server  | X  |  |
| LPR 連接埠監視器  | X  |  |
| 訊息佇列  | X  |  |
| 多重路徑 IO  | X  | X |
| 網路負載平衡  | X  | X |
| 對等名稱解析通訊協定  | X  |  |
| 高品質 Windows 音訊/視訊體驗  | X |  |
| 遠端協助  | X  |  |
| 遠端差異壓縮 | X  |  |
| 遠端伺服器管理工具  | X  |  |
| 卸除式存放裝置管理員 | X  | X |
| RPC Over HTTP Proxy | X  |  |
| 簡單 TCP/IP 服務  | X  |  |
| SMTP 伺服器  | X  |  |
| SMNP 服務  | X  | X  |
| SAN 存放管理員  | X  |  |
| UNIX 型應用程式子系統 | X | X  |
| Telnet 用戶端  | X | X  |
| Telnet 伺服器  | X   |  |
| TFTP 用戶端  | X   |  |
| Windows 內部資料庫  | X  |  |
| Windows PowerShell  | X  |  |
| Windows 產品啟用服務  | X   |  |
| Windows Server Backup 功能  | X  | X  |
| Windows 系統資源管理員  | X  |  |
| WINS 伺服器  | X | X |
| 無線區域網路服務 | X  |  |

同樣地，您必須知道有關 Server Core 上可用功能的一些重點：
* 某些功能可能需要特殊硬體，才能在 Server Core 上正常運作（或全部）。 這些功能包括 BitLocker 磁碟機加密、容錯移轉叢集、多重路徑 IO、網路負載平衡和卸除式存放裝置。 
* Standard Edition 不提供容錯移轉叢集。

## <a name="server-core-architecture"></a>伺服器核心架構
深入探討更深入探討 Server Core，讓我們先來看一下 Windows Server 2008 的 Server Core 安裝架構與完整安裝的比較。 首先，請記住 Server Core 不是不同版本的 Windows Server 2008，而只是在安裝 Windows Server 2008 到系統時，您可以選取的安裝選項。 這表示下列各項：
* Server Core 安裝上的核心與在相同硬體架構（x86 或 x64）和版本的完整安裝上找到的相同。 
* 如果二進位檔出現在 Server Core 安裝上，相同硬體架構（x86 或 x64）和版本的完整安裝會具有該特定二進位檔的相同版本（稍後會討論兩個例外狀況）。 
* 如果特定設定（例如特定的防火牆例外或特定服務的啟動類型）在 Server Core 安裝上有特定的預設設定，則在完整安裝相同的硬體架構（x86 或 x64）和版本時，會以完全相同的方式設定該設定。

圖1-3 顯示完整安裝和 Windows Server 2008 之 Server Core 安裝的架構的簡單觀點。 虛線表示伺服器核心的架構，而整個圖表則代表完整安裝的架構。 

此圖說明 Windows Server 2008 的模組化架構，而伺服器核心則是在核心作業系統功能的子集上進行構建。 在相同的硬體架構和版本中，全新安裝的伺服器核心上的每個檔案也會出現在完整安裝上，但只有兩個特殊檔案（Scregedit.wsf. manage-bde.wsf 和 Oclist）才會出現在伺服器核心上。 這些特殊檔案包含在 Server Core 上，可簡化伺服器核心安裝的初始設定，以及新增或移除角色和選擇性元件。 

![Server Core 和完整安裝的架構](../media/what-is-server-core-2008/Fig1-3.png)

**圖 1-3**Server Core 和完整安裝的架構

## <a name="driver-support"></a>驅動程式支援
如圖1-3 所示，伺服器核心的架構圖顯然很簡單;其中一件事不會顯示伺服器核心與完整安裝之間的設備磁碟機支援差異。 Windows Server 2008 的完整安裝包含適用于不同裝置類型的數千個內建驅動程式，可讓您在各種不同的硬體設定上安裝產品。 （Windows Vista 之類的用戶端作業系統包含更多的驅動程式，可支援通常不會與伺服器搭配使用的數位相機和掃描器等裝置）。 

如果新裝置連線到（或安裝于） Windows Server 2008 的完整安裝，隨插即用（PnP）子系統會先檢查裝置的內建驅動程式是否存在。 如果找到相容的內建驅動程式，PnP 子系統會自動安裝驅動程式，然後裝置便會運作。 在完整安裝的 Windows Server 2008 上，可能會顯示氣球快顯視窗通知，指出驅動程式已安裝，且裝置已準備好可供使用。 

在 Server Core 安裝上，驅動程式安裝程式會相同（在伺服器核心上有 PnP 子系統），並具有兩個資格。 首先，Server Core 僅包含最少的內建驅動程式，而且僅適用于下列裝置類型：
* 標準的影片圖形陣列（VGA）視頻驅動程式 
* 存放裝置的驅動程式 
* 網路介面卡的驅動程式

請注意，針對此處所示的三種裝置類別，Server Core 包含相同的內建驅動程式，可在對應的完整安裝上找到（針對相同的硬體架構）。 

此外，當 PnP 子系統自動安裝新裝置的驅動程式時，它會以無訊息方式執行-不會顯示任何氣球快顯視窗通知。 為什麼？ 因為 Server Core 上沒有 GUI，所以工作列上沒有任何通知區域！ 

那麼，當您將列印服務角色新增到 Server Core 安裝，而且想要安裝印表機時，該怎麼辦？ 您可以手動將印表機驅動程式新增到伺服器— Server Core 沒有任何內建的列印驅動程式。

## <a name="service-footprint"></a>服務足跡
因為 Server Core 是最小的安裝，所以它的系統服務使用量比相同硬體架構和版本的對應完整安裝更小。 例如，大約75系統服務預設會安裝在 Windows Server 2008 的完整安裝上，其大約50為自動啟動。 相較之下，Server Core 預設只會安裝70服務，而這些服務的啟動時間則不會超過40。 

[表 1-5] 列出 Server Core 安裝上預設安裝的服務，以及每個服務使用的啟動模式和帳戶。

**資料表 1-5**伺服器核心上預設安裝的系統服務

| 服務名稱  | 顯示名稱  | 啟動模式  | Account  |
| ------------- | ------------- | ------------ | ------------ |
| AeLookupSvc  | 應用程式體驗  | 自動 | LocalSystem |
| AppMgmt  | 應用程式管理  | 手動 | LocalSystem |
| BFE | Base Filtering Engine  | 自動 | LocalService |
| BITS | 背景智慧型傳送服務  | 自動 | LocalSystem |
| 瀏覽器 | 電腦瀏覽器  | 手動 | LocalSystem |
| CertPropSvc | 憑證傳播  | 手動 | LocalSystem |
| COMSysApp  | COM+ 系統應用程式  | 手動 | LocalSystem |
| CryptSvc  | 密碼編譯服務  | 自動 | 網路服務 |
| DcomLaunch  | DCOM 伺服器處理序啟動器  | 自動 | LocalSystem |
| Dhcp  | DHCP 用戶端  | 自動 | LocalService |
| Dnscache | DNS 用戶端  | 自動 | 網路服務 |
| DPS  | 診斷原則服務  | 自動 | LocalService |
| Eventlog | Windows 事件日誌  | 自動 | LocalService |
| EventSystem  | COM+ 事件系統  | 自動 | LocalService |
| FCRegSvc  | Microsoft 光纖通道平臺註冊服務  | 手動 | LocalService |
| gpsvc  | 群組原則用戶端  | 自動 | LocalSystem |
| hidserv | 人類介面裝置存取  | 手動 | LocalSystem |
| hkmsvc  | 健康情況金鑰和憑證管理  | 手動 | LocalSystem |
| IKEEXT  | IKE and AuthIP IPsec Keying Modules  | 自動 | LocalSystem |
| iphlpsvc  | IP 協助程式  | 自動 | LocalSystem |
| KeyIso | CNG 金鑰隔離  | 手動 | LocalSystem |
| KtmRm  | 用於分散式交易協調器的 KtmRm  | 自動 | 網路服務 |
| LanmanServer  | Server  | 自動 | LocalSystem |
| LanmanWorkstation  | Workstatione  | 自動 | LocalService |
| lltdsvc  | Link-Layer Topology Discovery Mapper  | 手動 | LocalService |
| lmhosts  | TCP/IP NetBIOS 協助程式  | 自動 | LocalService |
| MpsSvc  | Windows 防火牆  | 自動 | LocalService |
| MSDTC  | 分散式交易協調器  | 自動 | 網路服務 |
| MSiSCSI  | Microsoft iSCSI 啟動器服務  | 手動 | LocalSystem |
| msiserver  | Windows Installer  | 手動 | LocalSystem |
| napagent  | 網路存取保護代理程式  | 手動 | 網路服務 |
| Netlogon  | Netlogon  | 手動 | LocalSystem |
| netprofm  | 網路清單服務  | 自動 | LocalService |
| NlaSvc  | 網路定位知悉  | 自動 | 網路服務 |
| nsi  | 網路儲存介面服務  | 自動 | LocalService |
| pla  | 效能記錄及警示  | 手動 | LocalService |
| PlugPlay  | 隨插即用  | 自動 | LocalSystem |
| PolicyAgent  | IPSec 原則代理程式  | 自動 | 網路服務 |
| ProfSvc  | 使用者設定檔服務  | 自動 | LocalSystem |
| ProtectedStorage  | 受保護的儲存體  | 手動 | LocalSystem |
| RemoteRegistry  | 遠端登錄  | 自動 | LocalService |
| RpcSs  | 遠端程序呼叫 (RPC)  | 自動 | 網路服務 |
| RSoPProv | 原則結果組提供者  | 手動 | LocalSystem |
| sacsvr  | 特殊的管理主控台協助程式  | 手動 | LocalSystem |
| SamSs  | 安全性帳戶管理員  | 自動 | LocalSystem |
| SCardSvr | 智慧卡  | 手動 | LocalService |
| 排程 | 工作排程器  | 自動 | LocalSystem |
| SCPolicySvc | 智慧卡移除原則  | 手動 | LocalSystem |
| seclogon | 次要登入  | 自動 | LocalSystem |
| SENS | 系統事件通知服務  | 自動 | LocalSystem |
| SessionEnv | 終端機服務設定  | 手動 | LocalSystem |
| slsvc.exe  | 軟體授權 | 自動 | 網路服務 |
| SNMPTRAP  | SNMP 陷阱  | 手動 | LocalService |
| swprv  | Microsoft 軟體陰影複製提供者 | 手動 | LocalSystem |
| TB | TPM 基底服務  | 手動 | LocalService |
| TermService  | 終端機服務 | 自動 | 網路服務 |
| TrustedInstaller | Windows 模組安裝程式  | 自動 | LocalSystem |
| UmRdpService | 終端機服務 UserMode 埠重新導向程式  | 手動 | LocalSystem |
| vds | 虛擬磁碟  | 手動 | LocalSystem |
| VSS | 磁碟區陰影複製  | 手動 | LocalSystem |
| W32Time | Windows Time  | 自動 | LocalService |
| WcsPlugInService  | Windows 色彩系統  | 手動 | LocalService |
| WdiServiceHost  | 診斷服務裝載  | 手動 | LocalService |
| WdiSystemHost  | 診斷系統裝載  | 手動 | LocalSystem |
| Wecsvc | Windows 事件收集器  | 手動 | 網路服務 |
| WinHttpAuto-ProxySvc  | WinHTTP Web Proxy 自動探索服務  | 自動 | LocalService |
| Winmgmt | Windows Management Instrumentation | 自動 | LocalSystem |
| WinRM  | Windows 遠端管理 (WS-Management) | 自動 | 網路服務 |
| wmiApSrv  | WMI 效能配接器  | 手動 | LocalSystem |
| wuauserv | Windows Update | 自動 | LocalSystem |