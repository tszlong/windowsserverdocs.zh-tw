---
title: 角色、 角色服務和 Windows Server-Server Core 中包含的功能
description: Windows Server 的伺服器核心安裝選項中隨附何種角色和功能？
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 65729eb68c9590fd6316f5650be48f33c19c926d
ms.sourcegitcommit: 439edac95e1427086268cab469ed03b94db630da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "2217738"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>角色、 角色服務和 Windows Server-Server Core 中包含的功能

> 適用於： Windows Server （分號每年次通道） 和 Windows Server 2016

我們通常會談[功能的*不*在 Server Core](server-core-removed-roles.md) -現在我們將嘗試以不同的方法及要告訴您什麼是*包含*與某個項目是否*已安裝的預設值*。 下列角色、 角色服務和功能會*在*Windows Server 的伺服器核心安裝選項。 使用此資訊可協助找出 [伺服器核心] 選項如果適用於您的環境。 因為這是大型清單，請考慮針對特定角色或功能您感興趣-如果搜尋不會傳回您要尋找的搜尋未併入伺服器核心。

例如，如果您要搜尋的 「 遠端桌面工作階段主機"-您將不會發現此頁面上。 那是因為 RD 工作階段主機不包含在 Server Core 映像中。

請記住您可以[一律看起來](server-core-removed-roles.md)*不*會包含在項目。 這是只是不同的方法來查看事項。

## <a name="roles-included-in-server-core"></a>包含在 Server Core 的角色
[伺服器核心安裝選項包含下列伺服器角色。

| 角色                                            | 名稱                           | 預設安裝吗？ |
|-------------------------------------------------|--------------------------------|-----------------------|
| Active Directory 憑證服務           | AD 憑證                 | N                     |
| Active Directory 網域服務                | AD 網域服務             | N                     |
| Active Directory Federation Services            | ADFS 同盟                | N                     |
| Active Directory 輕量型目錄服務 | ADLDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| 裝置健康情況證明                       | DeviceHealthAttestationService | N                     |
| DHCP 伺服器                                     | DHCP                           | N                     |
| DNS 伺服器                                      | DNS                            | N                     |
| 檔案和存放服務                       | FileAndStorage 服務        | Y                     |
| 主機守護者服務                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| 列印和文件服務                     | 列印服務                 | N                     |
| 遠端存取                                   | 遠端存取                   | N                     |
| 遠端桌面服務                         | 遠端桌面服務        | N                     |
| 大量啟用服務                      | VolumeActivation               | N                     |
| 網頁伺服器 IIS                                  | 網頁伺服器                     | N                     |
| WindowsServer Essentials 體驗            | ServerEssentialsRole           | N                     |
| WindowsServer Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>包含在 Server Core 的角色服務
[伺服器核心安裝選項包含下列角色服務。

| 角色                                  | 角色服務                                                   | 名稱                    | 預設安裝吗？ |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Active Directory 憑證服務 | 憑證授權單位                                        | 授權 Cert ADC 單位     | N                     |
|                                       | 憑證註冊原則 Web 服務                      | ADC-註冊-Web 層處理     | N                     |
|                                       | 憑證註冊 Web 服務                             | ADC-註冊-Web-服務     | N                     |
|                                       | 憑證授權單位網頁註冊                         | ADC Web 註冊     | N                     |
|                                       | 網路裝置註冊服務                              | ADC 裝置註冊  | N                     |
|                                       | 線上回應程式                                               | ADC 線上 Cert        | N                     |
| Active Directory Rights Management    | Active Directory Rights Management Server                      | ADRMS 伺服器            | N                     |
|                                       | 識別身分同盟支援                                    | ADRMS 身分識別          | N                     |
| 檔案和存放服務             | 檔案和 iSCSI 服務                                        | 檔案服務           | N                     |
|                                       | 檔案伺服器                                                    | FS 檔案伺服器           | N                     |
|                                       | 網路檔案的 BranchCache                                  | FS BranchCache          | N                     |
|                                       | 重複資料刪除                                             | FS-資料-Deduplication   | N                     |
|                                       | DFS 命名空間                                                 | FS DFS 命名空間        | N                     |
|                                       | DFS 複寫                                                | FS DFS 複寫      | N                     |
|                                       | 檔案伺服器資源管理員                                   | FS 資源管理員     | N                     |
|                                       | 檔案伺服器 VSS 代理程式服務                                  | FS VSS 代理程式            | N                     |
|                                       | iSCSI Target Server                                            | iSCSITarget 伺服器      | N                     |
|                                       | iSCSI 目標儲存提供者 （VDS 和 VSS 硬體供應商） | iSCSITarget-VSS-VDS     | N                     |
|                                       | Server for NFS                                                 | FS NFS 服務          | N                     |
|                                       | 工作資料夾                                                   | FS SyncShareService     | N                     |
|                                       | 存放服務                                               | 儲存服務        | Y                     |
| 列印和文件服務           | 列印伺服器                                                   | 列印伺服器            | N                     |
|                                       | LPD 服務                                                    | 列印 LPD 服務       | N                     |
| 遠端存取                         | DirectAccess 和 VPN (RAS)                                     | DirectAccess VPN        | N                     |
|                                       | 路由                                                        | 路由                 | N                     |
|                                       | Web 應用程式 Proxy                                          | Web 應用程式 Proxy   | N                     |
| 遠端桌面服務               | 遠端桌面連線代理人                               | RDS 連線代理人   | N                     |
|                                       | 遠端桌面授權                                       | RDS 授權           | N                     |
|                                       | 遠端桌面虛擬化主機                             | RDS 虛擬化      | N                     |
| Web 伺服器 (IIS)                      | 網頁伺服器                                                     | Web WebServer           | N                     |
|                                       | 一般 HTTP 功能                                           | Web 常用 Http         | N                     |
|                                       | 預設文件                                               | 網頁預設文件         | N                     |
|                                       | 瀏覽目錄                                             | Web-Dir-瀏覽        | N                     |
|                                       | HTTP 錯誤                                                    | Web Http 錯誤         | N                     |
|                                       | 靜態內容                                                 | Web 靜態內容      | N                     |
|                                       | HTTP 重新導向                                               | Web Http 重新導向       | N                     |
|                                       | WebDAV 發佈                                              | Web DAV 發佈      | N                     |
|                                       | 狀況及診斷                                         | Web 健康情況              | N                     |
|                                       | HTTP 記錄                                                   | Web Http 記錄        | N                     |
|                                       | 自訂記錄                                                 | Web-自訂-記錄      | N                     |
|                                       | 記錄工具                                                  | Web-記錄文件庫       | N                     |
|                                       | ODBC 記錄                                                   | Web ODBC 記錄        | N                     |
|                                       | 要求監視器                                              | 網頁要求監視器     | N                     |
|                                       | 追蹤                                                        | Web-Http-追蹤        | N                     |
|                                       | 效能                                                    | 網頁效能         | N                     |
|                                       | 靜態內容壓縮                                     | Web-Stat-壓縮    | N                     |
|                                       | 動態內容壓縮                                    | Web-Dyn-壓縮     | N                     |
|                                       | 安全性                                                       | 網頁安全性            | N                     |
|                                       | 要求篩選                                              | Web 篩選           | N                     |
|                                       | 基本驗證                                           | Web 基本驗證          | N                     |
|                                       | 集中式的 SSL 憑證支援                            | Web CertProvider        | N                     |
|                                       | 用戶端憑證對應驗證                      | 網頁用戶端驗證         | N                     |
|                                       | 摘要式驗證                                          | Web 摘要式驗證         | N                     |
|                                       | IIS 用戶端憑證對應驗證                  | Web Cert 驗證           | N                     |
|                                       | IP 和網域限制                                     | Web IP 安全性         | N                     |
|                                       | URL 授權                                              | 網頁 Url 驗證            | N                     |
|                                       | Windows 驗證                                         | Web Windows 驗證        | N                     |
|                                       | 應用程式開發                                        | Web 應用程式開發             | N                     |
|                                       | .NET 擴充性 3.5                                         | Web-Net-Ext             | N                     |
|                                       | .NET 擴充性 4.6                                         | Web-Net-Ext45           | N                     |
|                                       | 應用程式初始化                                     | Web AppInit             | N                     |
|                                       | ASP （英文)                                                            | Asp 網頁 （英文)                 | N                     |
|                                       | ASP.NET 3.5                                                    | Net-web-asp （英文)             | N                     |
|                                       | ASP.NET 4.6                                                    | Web-asp （英文)-Net45           | N                     |
|                                       | CGI                                                            | Web CGI                 | N                     |
|                                       | ISAPI 擴充                                               | Web-ISAPI-Ext           | N                     |
|                                       | ISAPI 篩選器                                                  | Web ISAPI 篩選器        | N                     |
|                                       | 伺服器端包括                                           | 網頁包含            | N                     |
|                                       | WebSocket 通訊協定                                             | Web WebSockets          | N                     |
|                                       | FTP 伺服器                                                     | 網頁 Ftp 伺服器          | N                     |
|                                       | FTP 服務                                                    | Web Ftp 服務         | N                     |
|                                       | FTP 擴充性                                              | Web-Ftp-Ext             | N                     |
|                                       | 管理工具                                               | Web 管理工具          | N                     |
|                                       | IIS 6 管理相容性                                 | Web-管理-具         | N                     |
|                                       | IIS 6 Metabase 相容性                                   | Web Metabase            | N                     |
|                                       | IIS 6 指令碼工具                                          | Web Lgcy Scripting      | N                     |
|                                       | IIS 6 WMI 相容性                                        | Web WMI                 | N                     |
|                                       | IIS 管理指令碼與工具                               | Web 指令碼工具     | N                     |
|                                       | 管理服務                                             | Web 管理服務        | N                     |
| WindowsServer Update Services        | WID 連線                                               | UpdateServices WidDB    | N                     |
|                                       | WSUS 服務                                                  | UpdateServices 服務 | N                     |
|                                       | SQL Server 連線                                        | UpdateServices DB       | N                     |

## <a name="features-included-in-server-core"></a>包含在 [伺服器核心功能
[伺服器核心安裝選項包含下列功能。

| 功能                                                | 名稱                               | 預設安裝吗？ |
|--------------------------------------------------------|------------------------------------|-----------------------|
| .NET framework 3.5 功能                            | NET Framework 功能             | N                     |
| .NET framework 3.5 （包含.NET 2.0 和 3.0）       | NET Framework 核心                 | （移除）             |
| HTTP 啟動                                        | NET HTTP 啟動                | N                     |
| 非 HTTP 啟動                                    | NET-非-HTTP-加以啟動                 | N                     |
| .NET framework 4.6 功能                            | NET Framework 45-功能          | Y                     |
| .NET Framework 4.6                                     | NET Framework 45-核心              | Y                     |
| ASP.NET 4.6                                            | NET Framework 45-ASPNET            | N                     |
| WCF 服務                                           | NET-WCF-Services45                 | Y                     |
| HTTP 啟動                                        | NET WCF-HTTP Activation45          | N                     |
| 訊息佇列 (MSMQ) 啟用                      | NET WCF-MSMQ Activation45          | N                     |
| 啟用具名的管道                                  | NET WCF-管道 Activation45          | N                     |
| TCP 啟用                                         | NET WCF TCP-Activation45           | N                     |
| TCP 連接埠共用                                       | NET WCF TCP-PortSharing45          | Y                     |
| 背景智慧型傳送服務 (BITS)         | 位元                               | N                     |
| 壓縮伺服器                                         | 位元壓縮伺服器                | N                     |
| BitLocker 磁碟機加密                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Client for NFS                                         | NFS 用戶端                         | N                     |
| 容器                                             | 容器                         | N                     |
| 資料中心橋接                                   | 資料中心橋               | N                     |
| 增強的存放區                                       | EnhancedStorage                    | N                     |
| 容錯移轉叢集                                    | 容錯移轉叢集                | N                     |
| 群組原則管理                                | GPMC                               | N                     |
| I/O 服務品質                                 | DiskIo QoS                         | N                     |
| 可裝載 IIS 的 Web 核心                                  | Web WHC                            | N                     |
| IP 位址管理 (IPAM) 伺服器                    | IPAM                               | N                     |
| iSNS 伺服器服務                                    | ISNS                               | N                     |
| Management OData IIS 延伸模組                         | ManagementOdata                    | N                     |
| 媒體基礎                                       | 伺服器媒體基礎            | N                     |
| 訊息佇列                                        | MSMQ                               | N                     |
| 訊息佇列服務                               | MSMQ 服務                      | N                     |
| 訊息佇列伺服器                                 | MSMQ 伺服器                        | N                     |
| 目錄服務整合                          | MSMQ 目錄                     | N                     |
| HTTP 支援                                           | MSMQ HTTP 支援                  | N                     |
| 訊息佇列觸發程序                               | MSMQ 觸發程序                      | N                     |
| 路由服務                                        | MSMQ 路由                       | N                     |
| 訊息佇列 DCOM Proxy                             | MSMQ DCOM                          | N                     |
| 多重路徑 I/O                                          | 多重路徑 IO                       | N                     |
| MultiPoint 連線程式                                   | MultiPoint 連接器               | N                     |
| MultiPoint 連接器服務                          | MultiPoint 連接器服務      | N                     |
| MultiPoint 主管和 MultiPoint 儀表板            | MultiPoint 工具                   | N                     |
| 網路負載平衡                                 | NLB                                | N                     |
| 對等名稱解析通訊協定                          | PNRP                               | N                     |
| 高品質 Windows 音訊/視訊體驗                 | qWave                              | N                     |
| 遠端差異壓縮                        | RDC                                | N                     |
| 遠端伺服器管理工具                     | RSAT                               | N                     |
| 功能管理工具                           | RSAT 功能工具                 | N                     |
| BitLocker 磁碟加密管理公用程式  | RSAT 功能工具-BitLocker       | N                     |
| DataCenterBridging LLDP 工具                          | RSAT DataCenterBridging-LLDP 工具 | N                     |
| 容錯移轉叢集的工具                              | RSAT 叢集                    | N                     |
| Windows PowerShell 的容錯移轉叢集模組         | RSAT-Clustering-PowerShell         | N                     |
| 容錯移轉叢集自動化伺服器                     | RSAT 叢集 AutomationServer   | N                     |
| 容錯移轉叢集命令介面                     | RSAT 叢集 CmdInterface       | N                     |
| IP 位址 Management (IPAM) 用戶端                    | IPAM 用戶端功能                | N                     |
| 護套的 VM 工具                                      | RSAT-不但-VM-工具             | N                     |
| Windows PowerShell 的儲存複本模組          | RSAT-儲存空間-複本               | N                     |
| 角色系統管理工具                              | RSAT 角色工具                    | N                     |
| AD DS 與 AD LDS 工具                                 | RSAT AD 工具                      | N                     |
| Windows powershell 的 active Directory 模組         | RSAT AD PowerShell                 | N                     |
| AD DS 工具                                            | RSAT 新增                          | N                     |
| Active Directory 管理中心                 | RSAT-AD-AdminCenter                | N                     |
| AD DS 嵌入式管理單元和命令列工具                  | 就會新增工具                    | N                     |
| AD LDS 嵌入式管理單元和命令列工具                 | RSAT ADLDS                         | N                     |
| HYPER-V 管理工具                               | RSAT-Hyper-v-V-工具                 | N                     |
| Windows PowerShell 的 HYPER-V 模組                  | Hyper-V-PowerShell                 | N                     |
| Windows Server Update Services 工具                   | UpdateServices RSAT                | N                     |
| API 和 PowerShell 指令程式                             | UpdateServices API                 | N                     |
| DHCP 伺服器工具                                      | RSAT DHCP                          | N                     |
| DNS 伺服器工具                                       | RSAT DNS 伺服器                    | N                     |
| 遠端存取管理工具                         | RSAT 遠端存取                  | N                     |
| Windows PowerShell 的遠端存取模組            | RSAT 遠端存取 PowerShell       | N                     |
| RPC over HTTP Proxy                                    | RPC 移轉 HTTP-Proxy                | N                     |
| 安裝與開機事件集合                        | 安裝程式與-開機-事件-集合    | N                     |
| 簡單 TCP/IP 服務                                 | 簡單 tcpip]                       | N                     |
| SMB 1.0/CIFS 檔案共用支援                      | FS SMB1                            | Y                     |
| SMB 頻寬限制                                    | FS SMBBW                           | N                     |
| SNMP 服務                                           | SNMP 服務                       | N                     |
| SNMP WMI 提供者                                      | SNMP WMI 提供者                  | N                     |
| Telnet 用戶端                                          | Telnet 用戶端                      | N                     |
| 適用於網狀架構管理的 VM 防護工具               | FabricShieldedTools                | N                     |
| Windows 防禦者功能                              | Windows 防禦者功能          | Y                     |
| Windows Defender                                       | Windows 防禦者                   | Y                     |
| Windows 內部資料庫                              | Windows Internal Database          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5.1                                 | PowerShell                         | Y                     |
| Windows PowerShell 2.0 引擎                          | PowerShell V2                      | （移除）             |
| Windows PowerShell 想要設定 State Service | DSC 服務                        | N                     |
| Windows PowerShell Web Access                          | WindowsPowerShellWebAccess         | N                     |
| Windows 處理序啟用服務                     | 已                                | N                     |
| 程序模型                                          | 已處理序模型                  | N                     |
| .NET 環境 3.5                                   | 已 NET 環境                | N                     |
| 設定 api （英文)                                     | 是-Config-api （英文)                    | N                     |
| Windows Server Backup                                  | Windows Server Backup              | N                     |
| Windows Server 移轉工具                         | 移轉                          | N                     |
| Windows 標準式存放裝置管理             | WindowsStorageManagementService    | N                     |
| WinRM IIS 延伸模組                                    | WinRM-IIS-Ext                      | N                     |
| WINS 伺服器                                            | WINS                               | N                     |
| WoW64 支援                                          | WoW64 支援                      | Y                     |
|                                                        |                                    |                       |
