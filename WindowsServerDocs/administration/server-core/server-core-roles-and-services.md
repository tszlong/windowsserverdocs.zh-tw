---
title: Windows Server 中包含的角色、角色服務和功能-Server Core
description: Windows Server 的 Server Core 安裝選項包含哪些角色和功能？
ms.mktglfcycl: manage
ms.sitesec: library
author: pronichkin
ms.author: artemp
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: ded40a119d0d1ae759ec0b29ce9ee4808653ea50
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077755"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Windows Server 中包含的角色、角色服務和功能-Server Core

> 適用于： Windows Server 2019、Windows Server 2016 和 Windows Server (半年通道) 

我們通常會討論[*不*在 Server Core 中的內容](server-core-removed-roles.md)-現在我們要嘗試不同的方法，並告訴您有哪些內容 *，以及是否**預設安裝*了任何東西。 下列角色、角色服務和功能都是在 Windows Server 的 Server Core 安裝選項 *中* 。 使用此資訊來協助找出伺服器核心選項是否適用于您的環境。 因為這是大型清單，所以請考慮搜尋您感興趣的特定角色或功能。如果該搜尋不會傳回您要尋找的內容，則不會包含在 Server Core 中。

例如，如果您搜尋「遠端桌面工作階段主機」，就不會在此頁面上找到它。 這是因為 RD 工作階段主機並未包含在 Server Core 映射中。

請記住，您[一律可以查看](server-core-removed-roles.md)*未*包含的內容。 這只是查看事物的不同方式。

## <a name="roles-included-in-server-core"></a>Server Core 中包含的角色
Server Core 安裝選項包含下列伺服器角色。

| 角色                                            | 名稱                           | 預設會安裝？ |
|-------------------------------------------------|--------------------------------|-----------------------|
| Active Directory 憑證服務           | AD-Certificate                 | N                     |
| Active Directory Domain Services                | AD-網域服務             | N                     |
| Active Directory Federation Services            | ADFS-Federation                | N                     |
| Active Directory 輕量型目錄服務 | ADLDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| 裝置健康情況證明                       | DeviceHealthAttestationService | N                     |
| DHCP 伺服器                                     | DHCP                           | N                     |
| DNS 伺服器                                      | DNS                            | N                     |
| 檔案和存放服務                       | FileAndStorage-服務        | Y                     |
| 主機守護者服務                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| 列印和文件服務                     | Print-Services                 | N                     |
| 遠端存取                                   | RemoteAccess                   | N                     |
| 遠端桌面服務                         | 遠端桌面服務        | N                     |
| 大量啟用服務                      | Volumeactivation-full-role               | N                     |
| 網頁伺服器 (IIS)                                  | Web-Server                     | N                     |
| Windows Server Essentials 體驗            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>包含在 Server Core 中的角色服務
Server Core 安裝選項包含下列角色服務。

| 角色                                  | 角色服務                                                   | 名稱                    | 預設會安裝？ |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Active Directory 憑證服務 | 憑證授權單位                                        | ADCS-Cert-Authority     | N                     |
|                                       | 憑證註冊原則 Web 服務                      | ADC-註冊-Web-Pol     | N                     |
|                                       | 憑證註冊 Web 服務                             | ADC-註冊-Web-Svc     | N                     |
|                                       | 憑證授權單位網頁註冊。                         | ADC-Web-註冊     | N                     |
|                                       | 網路裝置註冊服務                              | ADC-裝置註冊  | N                     |
|                                       | 線上回應                                               | ADCS-Online-Cert        | N                     |
| Active Directory Rights Management    | Active Directory Rights Management Server                      | ADRMS-伺服器            | N                     |
|                                       | 識別身分同盟支援                                    | ADRMS-身分識別          | N                     |
| 檔案和存放服務             | 檔案和 iSCSI 服務                                        | 檔服務           | N                     |
|                                       | 檔案伺服器                                                    | FS-檔           | N                     |
|                                       | 網路檔案的 BranchCache                                  | FS-BranchCache          | N                     |
|                                       | 重複資料刪除                                             | FS-資料刪除   | N                     |
|                                       | DFS 命名空間                                                 | FS-DFS-Namespace        | N                     |
|                                       | DFS 複寫                                                | FS-DFS-Replication      | N                     |
|                                       | File Server Resource Manager                                   | FS-Resource-Manager     | N                     |
|                                       | 檔案伺服器 VSS 代理程式服務                                  | FS-VSS-代理程式            | N                     |
|                                       | iSCSI Target Server                                            | iSCSITarget-伺服器      | N                     |
|                                       | iSCSI 目標儲存提供者 (VDS 和 VSS 硬體提供者)  | iSCSITarget-VSS-VDS     | N                     |
|                                       | NFS 伺服器                                                 | FS-NFS-服務          | N                     |
|                                       | 工作資料夾                                                   | FS-SyncShareService     | N                     |
|                                       | 儲存體服務                                               | 儲存體服務        | Y                     |
| 列印和文件服務           | 列印伺服器                                                   | Print-Server            | N                     |
|                                       | LPD 服務                                                    | Print-LPD-Service       | N                     |
| 遠端存取                         | DirectAccess 和 VPN (RAS)                                      | DirectAccess-VPN        | N                     |
|                                       | 路由                                                        | 路由                 | N                     |
|                                       | Web 應用程式 Proxy                                          | Web 應用程式-Proxy   | N                     |
| 遠端桌面服務               | 遠端桌面連線代理人                               | RDS-連接-Broker   | N                     |
|                                       | 遠端桌面授權                                       | RDS-授權           | N                     |
|                                       | 遠端桌面虛擬主機                             | RDS-虛擬化      | N                     |
| Web 伺服器 (IIS)                      | Web 伺服器                                                     | Web-WebServer           | N                     |
|                                       | 一般 HTTP 功能                                           | Web-Common-Http         | N                     |
|                                       | 預設文件                                               | Web-Default-Doc         | N                     |
|                                       | 目錄瀏覽                                             | Web-Dir-Browsing        | N                     |
|                                       | HTTP 錯誤                                                    | Web-Http-Errors         | N                     |
|                                       | 靜態內容                                                 | Web-Static-Content      | N                     |
|                                       | HTTP 重新導向                                               | Web-Http-Redirect       | N                     |
|                                       | WebDAV 發行                                              | Web-DAV-發行      | N                     |
|                                       | 健康情況及診斷                                         | Web-Health              | N                     |
|                                       | HTTP 記錄                                                   | Web-Http-Logging        | N                     |
|                                       | 自訂記錄                                                 | Web-Custom-Logging      | N                     |
|                                       | 記錄工具                                                  | Web-Log-Libraries       | N                     |
|                                       | ODBC 記錄                                                   | Web-ODBC-Logging        | N                     |
|                                       | 要求監視器                                              | Web-Request-Monitor     | N                     |
|                                       | 追蹤                                                        | Web-Http-Tracing        | N                     |
|                                       | 效能                                                    | Web-Performance         | N                     |
|                                       | 靜態內容壓縮                                     | Web-Stat-Compression    | N                     |
|                                       | 動態內容壓縮                                    | Web-Dyn-Compression     | N                     |
|                                       | 安全性                                                       | Web-Security            | N                     |
|                                       | 要求篩選                                              | Web-Filtering           | N                     |
|                                       | 基本驗證                                           | Web-Basic-Auth          | N                     |
|                                       | 集中式 SSL 憑證支援                            | Web-CertProvider        | N                     |
|                                       | 用戶端憑證對應驗證                      | Web-Client-Auth         | N                     |
|                                       | 摘要式驗證                                          | Web-Digest-Auth         | N                     |
|                                       | IIS 用戶端憑證對應驗證                  | Web-Cert-Auth           | N                     |
|                                       | IP 及網域限制                                     | Web-IP-Security         | N                     |
|                                       | URL 授權                                              | Web-Url-Auth            | N                     |
|                                       | Windows 驗證                                         | Web-Windows-Auth        | N                     |
|                                       | 應用程式開發                                        | Web 應用程式-開發人員             | N                     |
|                                       | .NET 擴充性 3.5                                         | Web-Net-Ext             | N                     |
|                                       | .NET 擴充性4。6                                         | 網路-網路-Ext45           | N                     |
|                                       | 應用程式初始化                                     | Web-AppInit             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3.5                                                    | Web-Asp-Net             | N                     |
|                                       | ASP.NET 4。6                                                    | Web-Asp-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | ISAPI 擴充功能                                               | Web-ISAPI-Ext           | N                     |
|                                       | ISAPI 篩選                                                  | Web-ISAPI-Filter        | N                     |
|                                       | 伺服器端包含                                           | Web-Includes            | N                     |
|                                       | WebSocket 通訊協定                                             | Web-Websocket          | N                     |
|                                       | FTP 伺服器                                                     | Web-Ftp-Server          | N                     |
|                                       | FTP 服務                                                    | Web Ftp-服務         | N                     |
|                                       | FTP 擴充性                                              | Web Ftp-Ext             | N                     |
|                                       | 管理工具                                               | Web-Mgmt-Tools          | N                     |
|                                       | IIS 6 管理相容性                                 | Web-Mgmt-Compat         | N                     |
|                                       | IIS 6 Metabase 相容性                                   | Web-Metabase            | N                     |
|                                       | IIS 6 指令碼工具                                          | Web-Lgcy-Scripting      | N                     |
|                                       | IIS 6 WMI 相容性                                        | Web-WMI                 | N                     |
|                                       | IIS 管理指令碼及工具                               | Web-Scripting-Tools     | N                     |
|                                       | Management Service                                             | Web-Mgmt-Service        | N                     |
| Windows Server Update Services        | WID 連線能力                                               | UpdateServices-WidDB    | N                     |
|                                       | WSUS 服務                                                  | UpdateServices-服務 | N                     |
|                                       | SQL Server 連線能力                                        | UpdateServices-DB       | N                     |

## <a name="features-included-in-server-core"></a>Server Core 中包含的功能
Server Core 安裝選項包含下列功能。

| 功能                                                | 名稱                               | 預設會安裝？ |
|--------------------------------------------------------|------------------------------------|-----------------------|
| .NET Framework 3.5 功能                            | .NET Framework-功能             | N                     |
| .NET Framework 3.5 (包含 .NET 2.0 和 3.0)        | NET-Framework-Core                 |  (移除)              |
| HTTP 啟用                                        | NET-HTTP-Activation                | N                     |
| 非 HTTP 啟用                                    | NET-Non-HTTP-Activ                 | N                     |
| .NET Framework 4.6 功能                            | NET-Framework-45-功能          | Y                     |
| .NET Framework 4.6                                     | .NET Framework-45-核心              | Y                     |
| ASP.NET 4。6                                            | NET-Framework-45-ASPNET            | N                     |
| WCF Services                                           | NET-WCF-Services45                 | Y                     |
| HTTP 啟用                                        | NET-WCF-HTTP-Activation45          | N                     |
| 訊息佇列 (MSMQ) 啟用                      | NET-WCF-MSMQ-Activation45          | N                     |
| 具名管道啟用                                  | NET-WCF-管道-Activation45          | N                     |
| TCP 啟用                                         | NET-WCF-TCP-Activation45           | N                     |
| TCP 連接埠共用                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| 背景智慧型傳送服務 (BITS)         | BITS                               | N                     |
| Compact Server                                         | BITS-Compact-伺服器                | N                     |
| BitLocker 磁碟機加密                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Client for NFS                                         | NFS-用戶端                         | N                     |
| 容器                                             | 容器                         | N                     |
| 資料中心橋接                                   | 資料中心-橋接               | N                     |
| 增強的存放區                                       | EnhancedStorage                    | N                     |
| 容錯移轉叢集                                    | Failover-Clustering                | N                     |
| 群組原則管理                                | GPMC                               | N                     |
| I/O 服務品質                                 | DiskIo-QoS                         | N                     |
| 可裝載 IIS 的 Web 核心                                  | Web-WHC                            | N                     |
| IP 位址管理 (IPAM) 伺服器                    | IPAM                               | N                     |
| iSNS 伺服器服務                                    | ISNS                               | N                     |
| Management OData IIS 延伸模組                         | ManagementOdata                    | N                     |
| 媒體基礎                                       | 伺服器-媒體-基礎            | N                     |
| 訊息佇列                                        | MSMQ                               | N                     |
| 訊息佇列服務                               | MSMQ-Services                      | N                     |
| 訊息佇列伺服器                                 | MSMQ-Server                        | N                     |
| 目錄服務整合                          | MSMQ-Directory                     | N                     |
| HTTP 支援                                           | MSMQ-HTTP-Support                  | N                     |
| 訊息佇列觸發程序                               | MSMQ-Triggers                      | N                     |
| 路由服務                                        | MSMQ-Routing                       | N                     |
| 訊息佇列 DCOM Proxy                             | MSMQ-DCOM                          | N                     |
| 多重路徑 I/O                                          | Multipath-IO                       | N                     |
| MultiPoint 連線程式                                   | MultiPoint-連接器               | N                     |
| MultiPoint 連接器服務                          | MultiPoint-連接器-服務      | N                     |
| MultiPoint 管理員和 MultiPoint 儀表板            | MultiPoint-工具                   | N                     |
| Network Load Balancing                                 | NLB                                | N                     |
| 對等名稱解析通訊協定                          | PNRP                               | N                     |
| 高品質 Windows 音訊/視訊體驗                 | qWave                              | N                     |
| 遠端差異壓縮                        | RDC                                | N                     |
| 遠端伺服器管理工具                     | RSAT                               | N                     |
| 功能管理工具                           | RSAT-Feature-Tools                 | N                     |
| BitLocker 磁碟機加密管理公用程式  | RSAT-功能-工具-BitLocker       | N                     |
| DataCenterBridging LLDP Tools                          | RSAT-DataCenterBridging-LLDP-工具 | N                     |
| 容錯移轉叢集工具                              | RSAT-Clustering                    | N                     |
| Windows PowerShell 的容錯移轉叢集模組         | RSAT-Clustering-PowerShell         | N                     |
| 容錯移轉叢集 Automation 伺服器                     | RSAT-群集-AutomationServer   | N                     |
| 容錯移轉叢集命令介面                     | RSAT-群集-CmdInterface       | N                     |
| IP 位址管理 (IPAM) 用戶端                    | IPAM-用戶端功能                | N                     |
| 受防護的 VM 工具                                      | RSAT-受防護-VM-工具             | N                     |
| Windows PowerShell 的儲存體複本模組          | RSAT-儲存體-複本               | N                     |
| 角色管理工具                              | RSAT-Role-Tools                    | N                     |
| AD DS 和 AD LDS 工具                                 | RSAT-AD-工具                      | N                     |
| 適用於 Windows PowerShell 的 Active Directory 模組         | RSAT-AD-PowerShell                 | N                     |
| AD DS 工具                                            | RSAT-ADDS                          | N                     |
| Active Directory 管理中心                 | RSAT-AD-AdminCenter                | N                     |
| AD DS 嵌入式管理單元和命令列工具                  | RSAT-新增-工具                    | N                     |
| AD LDS 嵌入式管理單元和命令列工具                 | RSAT-ADLDS                         | N                     |
| Hyper-V 管理工具                               | RSAT-Hyper-v-工具                 | N                     |
| Windows PowerShell 的 Hyper-V 模組                  | Hyper-V-PowerShell                 | N                     |
| Windows Server Update Services 工具                   | UpdateServices-RSAT                | N                     |
| API 和 PowerShell Cmdlet                             | UpdateServices-API                 | N                     |
| DHCP 伺服器工具                                      | RSAT-DHCP                          | N                     |
| DNS 伺服器工具                                       | RSAT-DNS-Server                    | N                     |
| 遠端存取管理工具                         | RSAT-RemoteAccess                  | N                     |
| 適用於 Windows PowerShell 的遠端存取模組            | RSAT-RemoteAccess-PowerShell       | N                     |
| RPC over HTTP Proxy                                    | RPC-over-HTTP-Proxy                | N                     |
| 安裝並啟動事件收集                        | 設定和開機-事件收集    | N                     |
| 簡單 TCP/IP 服務                                 | Simple-TCPIP                       | N                     |
| SMB 1.0/CIFS 檔案共用支援                      | FS-SMB1                            | Y                     |
| SMB 頻寬限制                                    | FS-SMBBW                           | N                     |
| SNMP 服務                                           | SNMP-Service                       | N                     |
| SNMP WMI 提供者                                      | SNMP-WMI-Provider                  | N                     |
| Telnet 用戶端                                          | Telnet-Client                      | N                     |
| 適用於網狀架構管理的 VM 防護工具               | FabricShieldedTools                | N                     |
| Windows Defender 功能                              | Windows-Defender-功能          | Y                     |
| Windows Defender                                       | Windows-Defender                   | Y                     |
| Windows 內部資料庫                              | Windows-內部資料庫          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5.1                                 | PowerShell                         | Y                     |
| Windows PowerShell 2.0 引擎                          | PowerShell-V2                      |  (移除)              |
| Windows PowerShell Desired State Configuration 服務 | DSC-服務                        | N                     |
| Windows PowerShell Web 存取                          | WindowsPowerShellWebAccess         | N                     |
| Windows 處理序啟用服務                     | WAS                                | N                     |
| 處理序模型                                          | WAS-Process-Model                  | N                     |
| .NET 環境3。5                                   | WAS-NET-Environment                | N                     |
| 設定 API                                     | WAS-Config-APIs                    | N                     |
| Windows Server Backup                                  | Windows-伺服器-備份              | N                     |
| Windows Server 移轉工具                         | 遷移                          | N                     |
| Windows 標準式存放裝置管理             | WindowsStorageManagementService    | N                     |
| WinRM IIS 延伸模組                                    | WinRM-IIS-Ext                      | N                     |
| WINS 伺服器                                            | WINS                               | N                     |
| WoW64 支援                                          | WoW64-支援                      | Y                     |
|                                                        |                                    |                       |
