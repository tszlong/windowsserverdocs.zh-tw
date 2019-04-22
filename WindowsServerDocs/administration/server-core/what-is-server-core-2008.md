---
title: 什麼是 Server Core 2008？
description: 深入了解在 Windows Server 2008 的 Server Core 安裝選項
ms.prod: windows-server-threshold
ms.author: helohr
ms.date: 11/01/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: c1ef71dbc589cfdeac63b46d720c4bdd0a44dbaa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815399"
---
#<a name="what-is-server-core-2008"></a>什麼是 Server Core 2008？
>適用於：Windows Server 2008

>[!NOTE]
>這項資訊適用於 Windows Server 2008。 如需在 Windows Server 的 Server Core 的資訊，請參閱[什麼是 Server Core 安裝，在 Windows Server 中](https://docs.microsoft.com/windows-server/administration/server-core/what-is-server-core)。 

Server Core 選項是新的最小安裝選項時，使用您要部署 Windows Server 2008 的 Standard、 Enterprise 或 Datacenter 版本。 Server Core 為您提供最小安裝的 Windows Server 2008 支援安裝只有某些伺服器角色，如稍後這一章所述。 Windows Server 2008 支援安裝所有可用的伺服器角色及也其他 Microsoft 或協力廠商伺服器的應用程式，例如 Microsoft Exchange Server 或 SAP 的完整安裝選項與此相反。 

在我們繼續之前，片語 」 安裝選項 「 必須先加以說明。 一般來說，當您購買一份 Windows Server 2008，您可以購買使用特定版本或庫存單位 (Sku) 的授權。 表 1-1 會列出各版 Windows Server 2008 所提供。 此表也指出 （完整、 Server Core 或兩者） 的安裝選項可供每個版本。

**表 1-1** Windows Server 2008 版本和安裝選項的支援
| 版本       | 完整          | Server Core  |
| ------------- | :-------------: | :------------: |
| Windows Server 2008 Standard （x86 和 x64）       | X | X        |
| Windows Server 2008 Enterprise （x86 和 x64）       | X | X        |
| Windows Server 2008 Datacenter （x86 和 x64）        | X | X       |
| Windows Web Server 2008 （x86 和 x64）       | X | X  |
| Windows Server 2008 For Itanium 型系統       | X |     |
| Windows HPC Server 2008 (僅限 x64)       | X |   |
| Windows Server 2008 Standard without HYPER-V （x86 和 x64） | X | X |
| Windows Server 2008 Enterprise without HYPER-V （x86 和 x64）  | X | X |
| Windows Server 2008 Standard without HYPER-V （x86 和 x64） | X | X |

若要了解 」 安裝選項 」 是，假設您已購買大量授權，可讓您安裝 Windows Server 2008 Enterprise Edition 的複本。 當您插入系統中的大量授權媒體，並開始安裝程序，其中一個您所見，畫面示中圖 1-1，會顯示各種版本和安裝選項。

![選取要安裝的 Server Core 安裝選項](../media/what-is-server-core-2008/FIg1-1.png)

**圖 1-1**選取要安裝的 Server Core 安裝選項

在 圖 1-1，您的大量授權 （或產品金鑰，零售媒體） 可讓您可以選擇兩個安裝選項： 第二個選項 (完整安裝的 Windows Server 2008 Enterprise) 和第五個選項 (Server Core 安裝的 WindowsServer 2008 Enterprise)，以選取在此範例中，後者。 

##<a name="full-vs-server-core"></a>完全與Server Core 
Microsoft Windows 平台的早期年代，因為 Windows 伺服器已基本上是 全部內容包含所有類型的功能，其中有些您可能永遠不會實際使用您的網路環境中的伺服器。 比方說，當您在系統上安裝 Windows Server 2003，路由及遠端存取服務 (RRAS) 的二進位檔已安裝在伺服器上即使 （雖然您仍然必須設定並啟用 RRAS，才可以這樣做），會有這項服務不需要。 Windows Server 2008 會改善舊版安裝，您選擇在伺服器上安裝該特定角色時，才需要依伺服器角色的二進位檔。 不過，Windows Server 2008 的完整安裝選項仍會安裝許多服務和特定使用案例通常不需要其他元件。 

這是 Microsoft 建立的第二個安裝選項的原因 — Server Core — 適用於 Windows Server 2008： 若要排除任何服務和其他不重要，對於特定的支援常用的伺服器角色的功能。 例如，網域名稱系統 (DNS) 伺服器其實不需要安裝它，因為您不想要瀏覽網頁從 DNS 伺服器，基於安全性的 Windows Internet Explorer。 和 DNS 伺服器甚至不需要圖形化使用者介面 (GUI)，因為您可以管理 DNS 的各個層面，所以請從命令列使用強大的 Dnscmd.exe 命令，或從遠端使用 DNS Microsoft Management Console (MMC) 嵌入式管理單元。

若要避免這個問題，Microsoft 決定中刪除所有項目從已執行核心網路服務，例如 Active Directory 網域服務 (AD DS)、 DNS、 動態主機設定通訊協定 (DHCP)、 檔案和列印，並非絕對必要的 Windows Server 2008 和幾個其他伺服器角色。 結果會是新的 Server Core 安裝選項，可用來建立支援有限的數目的角色和功能的伺服器。 

##<a name="the-server-core-gui"></a>在 Server Core GUI
當您完成 Server Core 上安裝系統及登入第一次時，您會處於感到驚訝的位元。 圖 1-2 會顯示第一次登入之後的 Server Core 的使用者介面。

![Server Core 的使用者介面](../media/what-is-server-core-2008/Fig1-2.png)

**圖 1-2**伺服器核心使用者介面

沒有任何桌面 ！ 也就是沒有任何的 Windows 檔案總管殼層，使用其 [開始] 功能表、 工具列和其他您可能會看到使用的功能。 您只是工作的命令提示字元，這表示您不必執行大部分設定的 Server Core 安裝的其中一項輸入一次，也就是工作的慢，或使用指令碼和批次檔，可協助您加速和簡化您 configurati 命令透過自動化的工作。 您也可以執行使用回應檔案，當您執行自動的安裝 Server Core 的一些初始設定工作。 

提供給專精於使用 Netsh.exe、 Dfscmd.exe，等 Dnscmd.exe 命令列工具的系統管理員，設定及管理 Server Core 安裝可以是簡單、 甚至充滿樂趣。 對於使用者而言都不是專家，不過，這並非無可挽救。 您仍然可以使用標準的 Windows Server 2008 MMC 工具來管理 Server Core 安裝。 您只需要在執行 Windows Server 2008 的完整安裝，或是 Windows Vista Service Pack 1 的不同系統上使用它們。 

您將進一步了解設定及管理 Server Core 安裝中的章節 3 到 6 這本書中，而後面的章節則處理如何管理特定伺服器角色和其他元件。 若要深入了解各種不同的 Windows 命令列工具，以及如何使用它們，有兩個實用的資源，請您參閱：
* Windows Server 2008 Technical Library （） 命令參考 > 一節 
* *Windows 命令列 Administrator's Pocket Consultant*由 William R.Stanek (Microsoft Press,2008 年) 

表 1-2 會列出主要的 GUI 應用程式，以及其可執行檔，可在的 Server Core 安裝中使用。

**表 1-2** Server Core 安裝提供的 GUI 應用程式
| GUI 應用程式 | 可執行檔的路徑 |
| -------------   | -------------       | 
| 命令提示字元 | %WINDIR%\System32\Cmd.exe |
| Microsoft 支援服務診斷工具 | %WINDIR%\System32\MSdt.exe |
| 記事本 | %WINDIR%\System32\Notepad.exe |
| 登錄編輯程式 | %WINDIR%\System32\Regedt32.exe |
| 系統資訊 | %WINDIR%\System32\MSinfo32.exe |
| 工作管理員 | %WINDIR%\System32\Taskmgr.exe |
| Windows Installer | %WINDIR%\System32\MSiexec.exe |

這是相當的簡短清單 ！ 現在這裡是一份不包含在 Server Core 的使用者介面項目：
* Windows 檔案總管桌面 shell (Explorer.exe) 和任何支援的功能，例如佈景主題 
* 所有的 MMC 主控台 
* 所有的控制台中公用程式，除了 地區及語言選項 (Intl.cpl) 和日期和時間 (Timedate.cpl) 
* 所有的超文字標記語言 (HTML) 轉譯引擎，包括 Internet Explorer 和 HTML 說明 
* Windows Mail 
* Windows Media Player 
* 大部分的附屬應用程式，例如 [小畫家]、 [小算盤] 和 Wordpad

.NET Framework 也不是出現在 Server Core，這表示不支援 Server Core 安裝上執行受管理的程式碼。 只有原生程式碼 — 使用 Windows 應用程式開發介面 (Api) 所撰寫的程式碼，可以在 Server Core 上執行。 總而言之，任何 GUI 應用程式是依賴.NET Framework 或 Explorer.exe shell 不會執行 Server Core 上。 

>[!NOTE]
>因為 Windows PowerShell 需要.NET Framework，所以您無法安裝到 Server Core 上的 Windows PowerShell。 您可以不過，管理，只要您使用只有 PowerShell WMI 命令，從遠端使用 Windows PowerShell 的 Server Core 安裝。

##<a name="supported-server-roles"></a>支援的伺服器角色 
Server Core 安裝包含只在有限的相較於 Windows Server 2008 的完整安裝的伺服器角色。 表 1-3 比較適用於 Windows Server 2008 Enterprise edition 的完整和 Server Core 安裝的角色。 

**表 1-3**完整和 Server Core 安裝的 Windows Server 2008 Enterprise Edition 的伺服器角色的比較
| 伺服器角色  | 提供完整的安裝  | 用於 Server Core  |
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
| 「 UDDI 服務  | X  |  |
| Web 伺服器 (IIS) | X | X |
| Windows 部署服務  | X |  |

雖然適用於 Server Core 角色通常是不論架構 （x86 或 x64） 和產品版本相同，但有少數的例外狀況：
* HYPER-V （虛擬化） 角色是您購買 Windows Server 2008 與 HYPER-V 產品媒體時，才提供使用 (HYPER-V 是僅適用於 x64 版本)。 如果您不需要此角色，您可以購買 Windows Server 2008 HYPER-V 產品媒體不改為。 
* Standard Edition 中的 [檔案服務] 角色限制為一個獨立的分散式檔案系統 (DFS) 根目錄，並不支援交叉複寫 (DFS-R)。 
* 您可以在 Server Core 上安裝的串流處理媒體服務角色之前，您需要下載並安裝適當 Microsoft Update 獨立封裝 （.msu 檔案） 適用於您的伺服器架構 （x86 或 x64） 從 Microsoft 下載中心取得。
* 網頁伺服器 (IIS) 角色不支援 ASP.NET。 這是因為.NET Framework 不支援在 Server Core，這會限制您可以執行與伺服器核心網頁伺服器上。 

##<a name="supported-optional-features"></a>支援的選用功能
Server Core 安裝也支援 Windows Server 2008 的完整安裝上的有限的子集提供的功能。 表 1-4 比較適用於 Windows Server 2008 Enterprise edition 的完整和 Server Core 安裝的功能。

**表 1-4**的 Windows Server 2008 Enterprise edition 的完整和 Server Core 安裝的功能比較

| 功能  | 提供完整的安裝  | 用於 Server Core  |
| ------------- | :-------------: | :------------: |
| .NET framework 3.0 功能  | X  |  |
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
| 存放管理員 San  | X  |  |
| UNIX 應用程式子系統 | X | X  |
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

同樣地，有的幾點，您需要了解有關在 Server Core 上可用的功能：
* 有些功能可能需要特殊硬體運作正常 （或完全），在 Server Core 上。 這些功能包括 BitLocker 磁碟機加密、 容錯移轉叢集、 多重路徑 IO、 網路負載平衡，以及卸除式儲存體。 
* 容錯移轉叢集不是可在 Standard Edition 上取得。

##<a name="server-core-architecture"></a>伺服器核心架構
探究 Server Core，讓我們簡短探討 Windows Server 2008 的 Server Core 安裝的架構所使用的完整安裝進行比較。 首先，請記住，Server Core 不是不同版本的 Windows Server 2008，但只要安裝到系統的 Windows Server 2008 時，您可以選取的安裝選項。 這表示下列項目：
* Server Core 安裝上的核心是相同版本與相同的硬體架構 （x86 或 x64） 的完整安裝上找到。 
* 如果二進位檔已在的 Server Core 安裝上，相同的硬體架構 （x86 或 x64） 和版本的完整安裝具有相同版本的該特定二進位檔 （稍後討論的兩個例外狀況）。 
* 如果您的特定設定 （例如特定的防火牆例外狀況或特定服務的啟動類型） 都有特定的預設組態，在 Server Core 安裝上，設定該設定完全相同的方式在相同的完整安裝硬體架構 （x86 或 x64） 和版本。

圖 1-3 顯示簡單的檢視的完整安裝與 Windows Server 2008 的 Server Core 安裝的架構。 虛線表示的 Server Core 的架構，而使整個圖表代表完整安裝的架構。 

圖會說明 Windows Server 2008，在核心作業系統功能的子集時所建構的 Server Core 的模組化架構。 針對相同的硬體架構和版本，也是出現在完整的安裝中，除了兩個特殊檔案 （Scregedit.wsf 和 Oclist.exe），也就是只在 Server Core 上有全新的 Server Core 安裝上存在每個檔案。 若要簡化的初始組態的 Server Core 安裝和新增或移除角色和選用元件的 Server Core 上包含這些特殊的檔案。 

![在架構中的 Server Core 和完整安裝](../media/what-is-server-core-2008/Fig1-3.png)

**圖 1-3**的 Server Core 和完整安裝的架構

##<a name="driver-support"></a>驅動程式支援
很明顯地簡化架構圖顯示在 圖 1-3 中的 Server core;它不會顯示的一件事是裝置驅動程式支援在 Server Core 和完整安裝之間的差異。 Windows Server 2008 的完整安裝包含數千個不同類型的裝置，可讓您在各種不同的硬體組態上安裝產品的內建驅動程式。 （用戶端作業系統，例如 Windows Vista 包含更多的驅動程式，以支援裝置，例如數位相機及掃描器，通常不會與伺服器）。 

如果新的裝置是連線到 （或安裝於） Windows Server 2008 的完整安裝，隨插即用 (PnP) 子系統會先檢查是否存在於裝置內建驅動程式。 如果找到相容的內建驅動程式，則 PnP 子系統會自動安裝的驅動程式和裝置，然後操作。 在 Windows Server 2008 完整安裝中，快顯球形文字說明通知可能會顯示，表示已安裝驅動程式，裝置可供使用。 

在 Server Core 安裝上，驅動程式安裝程序都相同 （PnP 子系統是出現在 Server Core 上） 具有兩項資格。 首先，伺服器核心會包含只有最少的內建驅動程式，而且僅適用於下列類型的裝置：
* 標準視訊圖形陣列 (VGA) 視訊驅動程式 
* 儲存體裝置的驅動程式 
* 網路介面卡的驅動程式

請注意，針對每個如下所示的三個裝置類別，Server Core 包含對應的完整安裝 （適用於相同的硬體架構） 找到的相同內建驅動程式。 

此外，當 PnP 子系統會自動安裝新的裝置的驅動程式，它會以無訊息模式，會顯示任何提示快顯通知。 為什麼呢？ 因為 Server Core 上沒有 GUI 是沒有工作列中，因此在工作列上通知區域中沒有 ！ 

因此該怎麼辦當您新增到 Server Core 安裝的列印服務角色，且您想要安裝的印表機？ 您將印表機驅動程式以手動方式加入伺服器 — Server Core 有沒有內建的列印驅動程式。

##<a name="service-footprint"></a>服務使用量
因為 Server Core 是最小安裝，它會具有較小的系統服務使用量比對應的完整安裝，相同的硬體架構和版本。 例如，其中大約 50 會設定為自動啟動的 Windows Server 2008 的完整安裝上預設會安裝大約 75 部系統服務。 相較之下，Server Core 有大約只有 70 的服務會自動安裝的預設值，而且這些開始少於 40。 

表 1-5 列出在 Server Core 安裝上，使用的啟動模式的預設安裝的服務，每個服務所使用的帳戶。

**表 1-5**預設安裝在 Server Core 上的系統服務

| 服務名稱  | 顯示名稱  | 啟動模式  | 帳戶  |
| ------------- | ------------- | ------------ | ------------ |
| AeLookupSvc  | 應用程式體驗  | 自動 | LocalSystem |
| AppMgmt  | 應用程式管理  | Manual | LocalSystem |
| BFE | 篩選引擎的基底  | 自動 | LocalService |
| BITS | 背景智慧型傳送服務  | 自動 | LocalSystem |
| 瀏覽器 | 電腦瀏覽器  | Manual | LocalSystem |
| CertPropSvc | 憑證傳播  | Manual | LocalSystem |
| COMSysApp  | COM + 系統應用程式  | Manual | LocalSystem |
| CryptSvc  | 密碼編譯服務  | 自動 | Network-Service |
| DcomLaunch  | DCOM 伺服器處理序啟動器  | 自動 | LocalSystem |
| Dhcp  | DHCP 用戶端  | 自動 | LocalService |
| Dnscache | DNS 用戶端  | 自動 | Network-Service |
| DPS  | 診斷原則服務  | 自動 | LocalService |
| 事件記錄檔 | Windows 事件日誌  | 自動 | LocalService |
| EventSystem  | COM + 事件系統  | 自動 | LocalService |
| FCRegSvc  | Microsoft 光纖通道的平台註冊服務  | Manual | LocalService |
| gpsvc  | 群組原則用戶端  | 自動 | LocalSystem |
| hidserv | 人性化介面裝置存取  | Manual | LocalSystem |
| hkmsvc  | 健全狀況索引鍵和憑證管理  | Manual | LocalSystem |
| IKEEXT  | IKE 和 AuthIP IPsec 金鑰處理模組  | 自動 | LocalSystem |
| iphlpsvc  | IP 協助程式  | 自動 | LocalSystem |
| KeyIso | CNG 金鑰隔離  | Manual | LocalSystem |
| KtmRm  | 用於分散式交易協調器的 KtmRm  | 自動 | Network-Service |
| LanmanServer  | Server  | 自動 | LocalSystem |
| LanmanWorkstation  | Workstatione  | 自動 | LocalService |
| lltdsvc  | 連結階層拓樸探索對應  | Manual | LocalService |
| lmhosts  | TCP/IP NetBIOS 協助程式  | 自動 | LocalService |
| MpsSvc  | Windows 防火牆  | 自動 | LocalService |
| MSDTC  | 分散式交易協調器  | 自動 | Network-Service |
| MSiSCSI  | Microsoft iSCSI 啟動器服務  | Manual | LocalSystem |
| msiserver  | Windows Installer  | Manual | LocalSystem |
| napagent  | 網路存取保護代理程式  | Manual | Network-Service |
| Netlogon  | Netlogon  | Manual | LocalSystem |
| netprofm  | 網路清單服務  | 自動 | LocalService |
| NlaSvc  | 網路定位知悉  | 自動 | Network-Service |
| nsi  | Network Store Interface Service  | 自動 | LocalService |
| pla  | 效能記錄檔與警示  | Manual | LocalService |
| PlugPlay  | 隨插即用  | 自動 | LocalSystem |
| PolicyAgent  | IPSec 原則代理程式  | 自動 | Network-Service |
| ProfSvc  | 使用者設定檔服務  | 自動 | LocalSystem |
| ProtectedStorage  | 受保護的儲存體  | Manual | LocalSystem |
| RemoteRegistry  | 遠端登錄  | 自動 | LocalService |
| RpcSs  | 遠端程序呼叫 (RPC)  | 自動 | Network-Service |
| RSoPProv | 原則提供者原則結果組  | Manual | LocalSystem |
| sacsvr  | 特殊的系統管理主控台的協助程式  | Manual | LocalSystem |
| SamSs  | 安全性帳戶管理員  | 自動 | LocalSystem |
| SCardSvr | 智慧卡  | Manual | LocalService |
| 排程 | 工作排程器  | 自動 | LocalSystem |
| SCPolicySvc | 智慧卡移除原則  | Manual | LocalSystem |
| seclogon | 次要登入  | 自動 | LocalSystem |
| SENS | 系統事件通知服務  | 自動 | LocalSystem |
| SessionEnv | 終端機服務設定  | Manual | LocalSystem |
| slsvc  | 軟體授權 | 自動 | Network-Service |
| SNMPTRAP  | SNMP 陷阱  | Manual | LocalService |
| swprv  | Microsoft 軟體陰影複製提供者 | Manual | LocalSystem |
| TBS | TPM 基本服務  | Manual | LocalService |
| TermService  | 終端機服務 | 自動 | Network-Service |
| TrustedInstaller | Windows Modules Installer  | 自動 | LocalSystem |
| UmRdpService | Terminal Services UserMode Port Redirector  | Manual | LocalSystem |
| vds | 虛擬磁碟  | Manual | LocalSystem |
| VSS | 磁碟區陰影複製  | Manual | LocalSystem |
| W32Time | Windows Time  | 自動 | LocalService |
| WcsPlugInService  | Windows 色彩系統  | Manual | LocalService |
| WdiServiceHost  | 診斷服務主機  | Manual | LocalService |
| WdiSystemHost  | 診斷系統主應用程式  | Manual | LocalSystem |
| Wecsvc | Windows 事件收集器  | Manual | Network-Service |
| WinHttpAuto-ProxySvc  | WinHTTP Web Proxy 自動探索服務  | 自動 | LocalService |
| Winmgmt | Windows Management Instrumentation | 自動 | LocalSystem |
| WinRM  | Windows 遠端管理 (Ws-management) | 自動 | Network-Service |
| wmiApSrv  | WMI 效能配接器  | Manual | LocalSystem |
| wuauserv | Windows Update | 自動 | LocalSystem |