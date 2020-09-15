---
title: 什麼是 Server Core 2008？
description: 瞭解 Windows Server 2008 中的 Server Core 安裝選項
ms.date: 11/01/2017
ms.topic: article
author: pronichkin
ms.author: artemp
ms.openlocfilehash: 443c0307529a21a6a23da996486a2e6c40894af1
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077785"
---
# <a name="what-is-server-core-2008"></a>什麼是 Server Core 2008？
>適用于： Windows Server 2008

>[!NOTE]
>此資訊適用于 Windows Server 2008。 如需 Windows Server 中 Server Core 的詳細資訊，請參閱 [什麼是 Windows server 中的 Server Core 安裝](./what-is-server-core.md)。

當您部署 Standard、Enterprise 或 Datacenter edition 的 Windows Server 2008 時，Server Core 選項是新的最基本安裝選項。 Server Core 可讓您安裝 Windows Server 2008 的基本安裝，只支援安裝特定的伺服器角色，如本章稍後所述。 這與 Windows Server 2008 的完整安裝選項相較之下，它支援安裝所有可用的伺服器角色，以及其他 Microsoft 或協力廠商伺服器應用程式，例如 Microsoft Exchange Server 或 SAP。

在進一步討論之前，必須先說明「安裝選項」這一詞。 一般來說，當您購買一份 Windows Server 2008 時，您會購買授權，以使用特定版本或庫存單位 (Sku) 。 表格1-1 列出可用的各種 Windows Server 2008 版本。 此表格也會指出 (Full、Server Core 或兩個) 都可供每個版本使用的安裝選項。

**表 1-1** Windows Server 2008 版本及其對安裝選項的支援

| 版本       | 完整          | Server Core  |
| ------------- | :-------------: | :------------: |
| Windows Server 2008 Standard (x86 和 x64)        | X | X        |
| Windows Server 2008 Enterprise (x86 和 x64)        | X | X        |
| Windows Server 2008 Datacenter (x86 和 x64)         | X | X       |
| Windows Web Server 2008 (x86 和 x64)        | X | X  |
| 適用于 Itanium 型系統的 Windows Server 2008       | X |     |
| 僅限 Windows HPC Server 2008 (x64)        | X |   |
| 不含 Hyper-v 的 Windows Server 2008 Standard (x86 和 x64)  | X | X |
| 不含 Hyper-v 的 Windows Server 2008 Enterprise (x86 和 x64)   | X | X |
| 不含 Hyper-v 的 Windows Server 2008 Standard (x86 和 x64)  | X | X |

若要瞭解「安裝選項」，假設您已購買大量授權，可讓您安裝 Windows Server 2008 Enterprise Edition 的複本。 當您將大量授權的媒體插入系統並開始安裝程式時，您將會看到其中一個畫面，如 [圖 1-1] 所示，會提供您選擇的版本和安裝選項。

![選取要安裝的 Server Core 安裝選項](../media/what-is-server-core-2008/FIg1-1.png)

**圖 1-1** 選取要安裝的 Server Core 安裝選項

在圖1-1 中，零售媒體) 的大量授權 (或產品金鑰提供您兩種安裝選項：第二個選項 (完整安裝的 Windows Server 2008 Enterprise) 和第五個選項 (Windows server 2008 Enterprise) 的 Server Core 安裝，並在此範例中選取了後者。

## <a name="full-vs-server-core"></a>Full 與 Server Core
從 Microsoft Windows 平臺的早期幾天開始，Windows server 基本上都是包含各種功能的「一切」伺服器，其中有些是您可能永遠不會在網路環境中使用的。 比方說，當您在系統上安裝 Windows Server 2003 時，即使您不需要這項 (服務，也會在伺服器上安裝路由及遠端存取服務 (RRAS) 的二進位檔，即使您仍然必須先設定和啟用 RRAS 才能) 。 只有當您選擇在伺服器上安裝該特定角色時，Windows Server 2008 才會藉由安裝伺服器角色所需的二進位檔來改善舊版。 不過，Windows Server 2008 的完整安裝選項仍然會安裝許多服務和其他通常不需要用於特定使用案例的元件。

這就是 Microsoft 建立第二個安裝選項（Server Core）（適用于 Windows Server 2008）的原因：為了排除任何不必要的服務和其他功能，以支援某些常用的伺服器角色。 例如，網域名稱系統 (DNS) server 其實不需要安裝 Windows Internet Explorer，因為基於安全考慮，您不想要從 DNS 伺服器流覽網路。 DNS 伺服器甚至不需要圖形化使用者介面 (GUI) ，因為您可以從命令列使用強大的 Dnscmd.exe 命令，或從遠端使用 DNS Microsoft Management Console (MMC) 嵌入式管理單元來管理 DNS 的所有層面。

為了避免這種情況，Microsoft 決定在執行核心網路服務（例如 Active Directory Domain Services (AD DS) 、DNS、動態主機設定通訊協定 (DHCP) 、檔案和列印，以及其他幾個伺服器角色）時，從 Windows Server 2008 中找出所有不是絕對必要的專案。 結果是新的 Server Core 安裝選項，可用來建立只支援有限數目角色和功能的伺服器。

## <a name="the-server-core-gui"></a>Server Core GUI
當您在系統上完成安裝 Server Core 並第一次登入時，您會遇到一些驚喜。 圖1-2 顯示第一次登入之後的 Server Core 使用者介面。

![伺服器核心使用者介面](../media/what-is-server-core-2008/Fig1-2.png)

**圖 1-2** 伺服器核心使用者介面

沒有桌面！ 也就是說，沒有 Windows 檔案總管 shell，其 [開始] 功能表、工作列，以及您可以使用的其他功能。 您只需要一個命令提示字元，這表示您必須執行大部分的設定 Server Core 安裝的工作，方法是一次輸入一個命令、很慢，或是使用腳本和批次檔，這樣可協助您將設定工作自動化，以加速並簡化設定工作。 當您執行伺服器核心的自動安裝時，也可以使用回應檔案來執行一些初始設定工作。

如果系統管理員是使用命令列工具（例如 Netsh.exe、Dfscmd.exe 和 Dnscmd.exe）的專家，設定和管理 Server Core 安裝可能很簡單，甚至有趣。 不過，對於不是專家的人員來說，全部都不會遺失。 您仍然可以使用標準的 Windows Server 2008 MMC 工具來管理 Server Core 安裝。 您只需要在執行完整安裝 Windows Server 2008 或 Windows Vista Service Pack 1 的不同系統上使用它們。

您將在本書的第3章和第6章中，深入瞭解如何設定及管理 Server Core 安裝，而後續章節將討論如何管理特定的伺服器角色和其他元件。 若要深入瞭解各種 Windows 命令列工具，以及如何使用這些工具，有兩個很好的資源可以參考：
* Windows Server 2008 技術文件庫 ( # A1 的命令參考區段
* *Windows 命令列系統管理員的 Pocket 顧問* ，William Stanek (Microsoft 按下 2008) 

表格1-2 列出可在 Server Core 安裝中使用的主要 GUI 應用程式，以及其可執行檔。

**表 1-2** Server Core 安裝中提供的 GUI 應用程式

| GUI 應用程式 | 具有路徑的可執行檔 |
| -------------   | -------------       |
| 命令提示字元 | % WINDIR% \System32\Cmd.exe |
| Microsoft 支援服務診斷工具 | % WINDIR% \System32\MSdt.exe |
| 記事本 | % WINDIR% \System32\Notepad.exe |
| 登錄編輯程式 | % WINDIR% \System32\Regedt32.exe |
| 系統資訊 | % WINDIR% \System32\MSinfo32.exe |
| 工作管理員 | % WINDIR% \System32\Taskmgr.exe |
| Windows Installer | % WINDIR% \System32\MSiexec.exe |

這是非常簡短的清單！ 以下是 Server Core 未包含的使用者介面元素清單：
* Windows 檔案總管 desktop shell ( # A0) 以及任何支援的功能，例如主題
* 所有 MMC 主控台
* 所有主控台公用程式，但地區和語言選項除外 ( # A0) 和日期和時間 ( # A1) 
* 所有超文字標記語言 (HTML)  (HTML) 轉譯引擎，包括 Internet Explorer 和 HTML 說明
* Windows Mail
* Windows Media Player
* 大部分的配件，例如繪圖、計算機和 Wordpad

Server Core 中也不會有 .NET Framework，這表示不支援在 Server Core 安裝上執行 managed 程式碼。 只有原生程式碼（使用 Windows 應用程式開發介面所撰寫的程式碼 (Api) ）可以在 Server Core 上執行。 總而言之，相依于 .NET Framework 或 Explorer.exe shell 的任何 GUI 應用程式都不會在 Server Core 上執行。

>[!NOTE]
>因為 Windows PowerShell 需要 .NET Framework，所以您無法將 Windows PowerShell 安裝到 Server Core。 不過，您可以使用 Windows PowerShell 從遠端系統管理 Server Core 安裝，只要只使用 PowerShell WMI 命令即可。

## <a name="supported-server-roles"></a>支援的伺服器角色
相較于 Windows Server 2008 的完整安裝，Server Core 安裝只包含有限數量的伺服器角色。 表格1-3 比較 Windows Server 2008 Enterprise Edition 的完整安裝和 Server Core 安裝的可用角色。

**表 1-3** Windows Server 2008 Enterprise Edition 完整和 Server Core 安裝的伺服器角色比較

| 伺服器角色  | 適用于完整安裝  | 適用于 Server Core  |
| ------------- | :-------------: | :------------: |
| Active Directory 憑證服務 (AD CS)  | X |  |
| Active Directory Domain Services (AD DS)  | X  | X |
| Active Directory Federation Services (AD FS)  | X  |  |
| Active Directory 輕量型目錄服務 (AD LDS)  | X  | X |
| Active Directory Rights Management Services (AD RMS)  | X  |  |
| 應用程式伺服器  | X  |  |
| DHCP 伺服器  | X  | X |
| DNS 伺服器  | X  | X |
| 傳真伺服器  | X  |  |
| 檔案服務  | X  | X |
| Hyper-V  | X | X |
| Network Policy and Access Services  | X  |  |
| 列印服務  | X  | X |
| 串流處理媒體服務  | X  | X |
| 終端機服務  | X  |  |
| UDDI 服務  | X  |  |
| Web 伺服器 (IIS) | X | X |
| Windows Deployment Services  | X |  |

無論 (x86 或 x64) 和產品版本的架構，適用于 Server Core 的角色通常都相同，但有一些例外狀況：
* 只有當您購買 Windows Server 2008 與 Hyper-v 產品媒體時，才可使用 Hyper-v (虛擬化) 角色 (Hyper-v 僅適用于) 的 x64 版本。 如果您不需要此角色，則可以改為購買 Windows Server 2008 （不含 Hyper-v 產品媒體）。
* Standard Edition 上的檔案服務角色受限於一個獨立分散式檔案系統 (DFS) 根目錄，且不支援 (DFS-R) 的跨檔案複寫。
* 在 Server Core 上安裝串流處理媒體服務角色之前，您必須先從 Microsoft 下載中心下載並安裝適合您伺服器架構的 Microsoft Update 獨立套件 ( .msu 檔案)  (x86 或 x64) 。
* Web 服務器 (IIS) 角色不支援 ASP.NET。 這是因為 Server Core 並不支援 .NET Framework，這會限制您可以使用 Server Core Web 服務器進行的作業。

## <a name="supported-optional-features"></a>支援的選用功能
Server Core 安裝也只支援 Windows Server 2008 完整安裝上提供的有限功能子集。 表1-4 比較 Windows Server 2008 Enterprise Edition 的完整安裝和 Server Core 安裝可用的功能。

**表 1-4** Windows Server 2008 Enterprise Edition 完整和 Server Core 安裝的功能比較

| 功能  | 適用于完整安裝  | 適用于 Server Core  |
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
| Network Load Balancing  | X  | X |
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

同樣地，您需要知道有關 Server Core 可用功能的一些重點：
* 某些功能可能需要特殊硬體才能正常運作 (或伺服器核心上的所有) 。 這些功能包括 BitLocker 磁碟機加密、容錯移轉叢集、多重路徑 IO、網路負載平衡和卸除式存放裝置。
* Standard Edition 無法使用容錯移轉叢集。

## <a name="server-core-architecture"></a>伺服器核心架構
更深入深入探討 Server Core，讓我們透過將 Windows Server 2008 的 Server Core 安裝與完整安裝的架構進行比較，以簡單的方式查看。 首先，請記住，Server Core 不是不同版本的 Windows Server 2008，而是您在將 Windows Server 2008 安裝到系統時，可以選取的安裝選項。 這具有如下表示：
* Server Core 安裝上的核心與在相同硬體架構的完整安裝 (x86 或 x64) 和 edition 中找到的相同。
* 如果伺服器核心安裝上有二進位檔，則相同硬體架構 (x86 或 x64) 和 edition 的完整安裝會與該特定二進位 (的相同版本，) 稍後討論的兩個例外狀況。
* 如果特定的設定 (例如，特定的防火牆例外或特定服務的啟動類型) 在 Server Core 安裝上有特定的預設設定，則在完整安裝相同硬體架構 (x86 或 x64) 和版本時，會以完全相同的方式設定該設定。

圖1-3 顯示完整安裝和 Server Core 安裝的 Windows Server 2008 架構的簡化觀點。 虛線表示 Server Core 的架構，而整個圖表則代表完整安裝的架構。

下圖說明 Windows Server 2008 的模組化架構，其中 Server Core 是以核心作業系統功能的子集來建造。 針對相同的硬體架構和版本，全新安裝的 Server Core 上的每個檔案也都存在於完整安裝中，但兩個特殊檔案除外 (Scregedit.wsf w2kmiguser.wsf 和 Oclist.exe) ，這些檔案只存在於 Server Core 上。 這些特殊檔案包含在 Server Core 上，以簡化伺服器核心安裝的初始設定，以及新增或移除角色和選用元件。

![Server Core 和完整安裝的架構](../media/what-is-server-core-2008/Fig1-3.png)

**圖 1-3** Server Core 和完整安裝的架構

## <a name="driver-support"></a>驅動程式支援
[圖 1-3] 所示的伺服器核心架構圖表顯然很簡單;其中一件不會顯示的是 Server Core 與完整安裝之間的設備磁碟機支援差異。 Windows Server 2008 的完整安裝包含了數千個適用于不同裝置類型的現成驅動程式，可讓您在各種不同的硬體設定上安裝產品。  (的用戶端作業系統（例如 Windows Vista）包含更多的驅動程式，以支援通常不會與伺服器搭配使用的數位相機和掃描器等裝置。 ) 

如果新的裝置已連線到 (或安裝于) 完整安裝 Windows Server 2008，隨插即用 (PnP) 子系統會先檢查裝置的內建驅動程式是否存在。 如果找到相容的內建驅動程式，PnP 子系統會自動安裝驅動程式，然後再操作裝置。 在完整安裝的 Windows Server 2008 上，可能會顯示氣球快顯視窗通知，指出驅動程式已安裝，且裝置已可供使用。

在 Server Core 安裝上，驅動程式安裝程式是相同的 (在 Server Core) 上有兩個合格的 PnP 子系統。 首先，Server Core 只包含最少的現成驅動程式，且僅適用于下列類型的裝置：
* 標準 Video 圖形陣列 (VGA) Video 驅動程式
* 存放裝置的驅動程式
* 網路介面卡的驅動程式

請注意，在這裡顯示的三個裝置類別中，Server Core 會包含相同的內建驅動程式，這些驅動程式可在相同硬體架構) 的對應完整安裝 (上找到。

此外，當 PnP 子系統自動為新裝置安裝驅動程式時，它會以無訊息方式執行，並不會顯示任何提示快顯視窗通知。 為什麼不用？ 因為 Server Core 上沒有 GUI，所以沒有任何工作列，所以工作列上沒有通知區域！

那麼當您將列印服務角色新增至 Server Core 安裝，而且想要安裝印表機時，該怎麼辦？ 您可以手動將印表機驅動程式新增至伺服器— Server Core 沒有任何現成的列印驅動程式。

## <a name="service-footprint"></a>服務足跡
因為 Server Core 是最小的安裝，所以它的系統服務使用量較小，與相同硬體架構和版本的對應完整安裝相同。 例如，大約75系統服務預設會安裝在 Windows Server 2008 的完整安裝上，大約50會設定為自動啟動。 相40較之下，Server Core 預設只會安裝70的服務，而這些服務會自動啟動。

表1-5 列出 Server Core 安裝上預設安裝的服務，以及每個服務所使用的啟動模式和帳戶。

**表 1-5** 預設安裝在 Server Core 上的系統服務

| 服務名稱  | 顯示名稱  | 啟動模式  | 帳戶  |
| ------------- | ------------- | ------------ | ------------ |
| AeLookupSvc  | 應用程式體驗  | 自動 | LocalSystem |
| AppMgmt  | 應用程式管理  | 手動 | LocalSystem |
| BFE | 基礎篩選引擎  | 自動 | LocalService |
| BITS | 背景智慧型傳送服務  | 自動 | LocalSystem |
| 瀏覽器 | 電腦瀏覽器  | 手動 | LocalSystem |
| CertPropSvc | 憑證傳播  | 手動 | LocalSystem |
| COMSysApp  | COM+ 系統應用程式  | 手動 | LocalSystem |
| CryptSvc  | 密碼編譯服務  | 自動 | 網路服務 |
| DcomLaunch  | DCOM 伺服器處理序啟動器  | 自動 | LocalSystem |
| Dhcp  | DHCP 用戶端  | 自動 | LocalService |
| Dnscache | DNS 用戶端  | 自動 | 網路服務 |
| DPS  | 診斷原則服務  | 自動 | LocalService |
| 事件 | Windows 事件記錄檔  | 自動 | LocalService |
| EventSystem  | COM+ 事件系統  | 自動 | LocalService |
| FCRegSvc  | Microsoft 光纖通道 Platform 註冊服務  | 手動 | LocalService |
| gpsvc  | 群組原則用戶端  | 自動 | LocalSystem |
| hidserv | 人力介面裝置存取  | 手動 | LocalSystem |
| hkmsvc  | 健康情況金鑰和憑證管理  | 手動 | LocalSystem |
| IKEEXT  | IKE 和 AuthIP IPsec 金鑰處理模組  | 自動 | LocalSystem |
| iphlpsvc  | IP 協助程式  | 自動 | LocalSystem |
| KeyIso | CNG 金鑰隔離  | 手動 | LocalSystem |
| KtmRm  | 用於分散式交易協調器的 KtmRm  | 自動 | 網路服務 |
| LanmanServer  | 伺服器  | 自動 | LocalSystem |
| LanmanWorkstation  | Workstatione  | 自動 | LocalService |
| lltdsvc  | 連結階層拓撲探索對應程式  | 手動 | LocalService |
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
| slsvc  | 軟體授權 | 自動 | 網路服務 |
| SNMPTRAP  | SNMP 陷阱  | 手動 | LocalService |
| swprv  | Microsoft 軟體陰影複製提供者 | 手動 | LocalSystem |
| Tbs | TPM 基本服務  | 手動 | LocalService |
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