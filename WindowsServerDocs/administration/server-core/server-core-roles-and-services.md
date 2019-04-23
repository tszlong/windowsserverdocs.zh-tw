---
title: 角色、 角色服務和功能包含在 Windows Server 的 Server Core
description: 在 Windows Server 的 Server Core 安裝選項中包含哪些角色和功能？
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 65729eb68c9590fd6316f5650be48f33c19c926d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859289"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>角色、 角色服務和功能包含在 Windows Server 的 Server Core

> 適用於：Windows Server （半年通道） 和 Windows Server 2016

通常談論[功能的*不*在 Server Core](server-core-removed-roles.md) -現在我們要嘗試不同的方法，並告訴您什麼的*包含*以及項目是否*預設會安裝*。 下列角色、 角色服務和功能*在*Windows Server 的 Server Core 安裝選項。 您可以使用此資訊來協助找出如果 Server Core 選項適用於您的環境。 因為這是大型清單，請考慮搜尋特定的角色或功能，您有興趣-如果該搜尋不會傳回您要尋找的它將不會包含在 Server Core。

例如，如果您搜尋 「 遠端桌面工作階段主機 」-您將不會發現此頁面上。 這是因為 RD 工作階段主機不會包含在 Server Core 映像。

請記住，您可以[一律會尋找](server-core-removed-roles.md)瞧的*不*包含。 這是只是不同的方法來看看事情。

## <a name="roles-included-in-server-core"></a>包含在 Server Core 中的角色
Server Core 安裝選項包含下列伺服器角色。

| [角色]                                            | 名稱                           | 預設會安裝嗎？ |
|-------------------------------------------------|--------------------------------|-----------------------|
| Active Directory 憑證服務           | AD-Certificate                 | N                     |
| Active Directory Domain Services                | AD-Domain-Services             | N                     |
| Active Directory 同盟服務            | ADFS-Federation                | N                     |
| Active Directory 輕量型目錄服務 | ADLDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| 裝置健康情況證明                       | DeviceHealthAttestationService | N                     |
| DHCP 伺服器                                     | DHCP                           | N                     |
| DNS 伺服器                                      | DNS                            | N                     |
| 檔案和存放服務                       | FileAndStorage-Services        | Y                     |
| 主機守護者服務                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| 列印和文件服務                     | Print-Services                 | N                     |
| 遠端存取                                   | RemoteAccess                   | N                     |
| 遠端桌面服務                         | Remote-Desktop-Services        | N                     |
| 大量啟用服務                      | VolumeActivation               | N                     |
| 網頁伺服器 (IIS)                                  | Web-Server                     | N                     |
| Windows Server Essentials 體驗            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>包含在 Server Core 中的角色服務
Server Core 安裝選項包括下列角色服務。

| [角色]                                  | 角色服務                                                   | 名稱                    | 預設會安裝嗎？ |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Active Directory 憑證服務 | 憑證授權單位                                        | ADCS-Cert-Authority     | N                     |
|                                       | 憑證註冊原則 Web 服務                      | ADCS-Enroll-Web-Pol     | N                     |
|                                       | 憑證註冊 Web 服務                             | ADCS-Enroll-Web-Svc     | N                     |
|                                       | 憑證授權單位網頁註冊。                         | ADCS-Web-Enrollment     | N                     |
|                                       | 網路裝置註冊服務                              | ADCS-Device-Enrollment  | N                     |
|                                       | 線上回應                                               | ADCS-Online-Cert        | N                     |
| Active Directory Rights Management    | Active Directory Rights Management Server                      | ADRMS-Server            | N                     |
|                                       | 識別身分同盟支援                                    | ADRMS-Identity          | N                     |
| 檔案和存放服務             | 檔案和 iSCSI 服務                                        | File-Services           | N                     |
|                                       | 檔案伺服器                                                    | FS-FileServer           | N                     |
|                                       | 網路檔案的 BranchCache                                  | FS-BranchCache          | N                     |
|                                       | 重複資料刪除                                             | FS-Data-Deduplication   | N                     |
|                                       | DFS 命名空間                                                 | FS-DFS-Namespace        | N                     |
|                                       | DFS 複寫                                                | FS-DFS-Replication      | N                     |
|                                       | 檔案伺服器資源管理員                                   | FS-Resource-Manager     | N                     |
|                                       | 檔案伺服器 VSS 代理程式服務                                  | FS-VSS-Agent            | N                     |
|                                       | iSCSI 目標伺服器                                            | iSCSITarget-Server      | N                     |
|                                       | iSCSI 目標存放提供者 （VDS 和 VSS 硬體提供者） | iSCSITarget-VSS-VDS     | N                     |
|                                       | Server for NFS                                                 | FS-NFS-Service          | N                     |
|                                       | 工作資料夾                                                   | FS-SyncShareService     | N                     |
|                                       | 存放服務                                               | Storage-Services        | Y                     |
| 列印和文件服務           | 列印伺服器                                                   | Print-Server            | N                     |
|                                       | LPD 服務                                                    | Print-LPD-Service       | N                     |
| 遠端存取                         | DirectAccess 和 VPN (RAS)                                     | DirectAccess-VPN        | N                     |
|                                       | 路由                                                        | 路由                 | N                     |
|                                       | Web 應用程式 Proxy                                          | Web-Application-Proxy   | N                     |
| 遠端桌面服務               | 遠端桌面連線代理人                               | RDS-Connection-Broker   | N                     |
|                                       | 遠端桌面授權                                       | RDS 授權           | N                     |
|                                       | 遠端桌面虛擬主機                             | RDS-Virtualization      | N                     |
| Web 伺服器 (IIS)                      | Web 伺服器                                                     | Web-WebServer           | N                     |
|                                       | 一般 HTTP 功能                                           | Web-Common-Http         | N                     |
|                                       | 預設文件                                               | Web-Default-Doc         | N                     |
|                                       | 瀏覽目錄                                             | -Dir-瀏覽網頁        | N                     |
|                                       | HTTP 錯誤                                                    | Web-Http-Errors         | N                     |
|                                       | 靜態內容                                                 | Web-Static-Content      | N                     |
|                                       | HTTP 重新導向                                               | Web-Http-Redirect       | N                     |
|                                       | WebDAV 發行                                              | Web DAV 發行      | N                     |
|                                       | 狀況及診斷                                         | Web 健康狀態              | N                     |
|                                       | HTTP 記錄                                                   | Web-Http-Logging        | N                     |
|                                       | 自訂記錄                                                 | Web 自訂記錄      | N                     |
|                                       | 記錄工具                                                  | Web 記錄程式庫       | N                     |
|                                       | ODBC 記錄                                                   | Web-ODBC-Logging        | N                     |
|                                       | 要求監視器                                              | Web-Request-Monitor     | N                     |
|                                       | 追蹤                                                        | Web-Http-Tracing        | N                     |
|                                       | 效能                                                    | Web 效能         | N                     |
|                                       | 靜態內容壓縮                                     | Web-Stat-Compression    | N                     |
|                                       | 動態內容壓縮                                    | Web 年的 Dyn 壓縮     | N                     |
|                                       | 安全性                                                       | Web-Security            | N                     |
|                                       | Request Filtering                                              | Web 篩選           | N                     |
|                                       | 基本驗證                                           | Web-Basic-Auth          | N                     |
|                                       | 集中式的 SSL 憑證支援                            | Web-CertProvider        | N                     |
|                                       | 用戶端憑證對應驗證                      | Web-Client-Auth         | N                     |
|                                       | 摘要式驗證                                          | Web-Digest-Auth         | N                     |
|                                       | IIS 用戶端憑證對應驗證                  | Web-Cert-Auth           | N                     |
|                                       | IP 及網域限制                                     | Web-IP-Security         | N                     |
|                                       | URL 授權                                              | Web-Url-Auth            | N                     |
|                                       | Windows 驗證                                         | Web-Windows-Auth        | N                     |
|                                       | 應用程式開發                                        | Web-App-Dev             | N                     |
|                                       | .NET 擴充性 3.5                                         | Web-Net-Ext             | N                     |
|                                       | .NET 擴充性 4.6                                         | Web-Net-Ext45           | N                     |
|                                       | 應用程式初始化                                     | Web-AppInit             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3.5                                                    | Web-Asp-Net             | N                     |
|                                       | ASP.NET 4.6                                                    | Web-Asp-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | ISAPI 擴充程式                                               | Web-ISAPI-Ext           | N                     |
|                                       | ISAPI Filters                                                  | Web-ISAPI-Filter        | N                     |
|                                       | 伺服器端包含                                           | 網頁包含            | N                     |
|                                       | WebSocket 通訊協定                                             | Web-WebSockets          | N                     |
|                                       | FTP 伺服器                                                     | Web-Ftp-Server          | N                     |
|                                       | FTP 服務                                                    | Web-Ftp-Service         | N                     |
|                                       | FTP 擴充性                                              | Web-Ftp-Ext             | N                     |
|                                       | 管理工具                                               | Web-Mgmt-Tools          | N                     |
|                                       | IIS 6 管理相容性                                 | Web-Mgmt-Compat         | N                     |
|                                       | IIS 6 Metabase 相容性                                   | Web-Metabase            | N                     |
|                                       | IIS 6 指令碼工具                                          | Web-Lgcy-Scripting      | N                     |
|                                       | IIS 6 WMI 相容性                                        | Web-WMI                 | N                     |
|                                       | IIS 管理指令碼及工具                               | Web-Scripting-Tools     | N                     |
|                                       | 管理服務                                             | Web-Mgmt-Service        | N                     |
| Windows Server Update Services        | WID 連接                                               | UpdateServices-WidDB    | N                     |
|                                       | WSUS 服務                                                  | UpdateServices-Services | N                     |
|                                       | SQL Server 連接性                                        | UpdateServices-DB       | N                     |

## <a name="features-included-in-server-core"></a>包含在 Server Core 的功能
Server Core 安裝選項包括下列功能。

| 功能                                                | 名稱                               | 預設會安裝嗎？ |
|--------------------------------------------------------|------------------------------------|-----------------------|
| .NET framework 3.5 功能                            | NET-Framework-Features             | N                     |
| .NET framework 3.5 （包括.NET 2.0 和 3.0）       | NET-Framework-Core                 | （移除）             |
| HTTP 啟動                                        | NET-HTTP-Activation                | N                     |
| 非HTTP 啟動                                    | NET-Non-HTTP-Activ                 | N                     |
| .NET framework 4.6 功能                            | NET-Framework-45-Features          | Y                     |
| .NET Framework 4.6                                     | NET-Framework-45-Core              | Y                     |
| ASP.NET 4.6                                            | NET-Framework-45-ASPNET            | N                     |
| WCF 服務                                           | NET-WCF-Services45                 | Y                     |
| HTTP 啟動                                        | NET-WCF-HTTP-Activation45          | N                     |
| 訊息佇列 (MSMQ) 啟動                      | NET-WCF-MSMQ-Activation45          | N                     |
| 具名的管道啟動                                  | NET-WCF-Pipe-Activation45          | N                     |
| TCP 啟動                                         | NET-WCF-TCP-Activation45           | N                     |
| TCP 連接埠共用                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| 背景智慧型傳送服務 (BITS)         | BITS                               | N                     |
| Compact Server                                         | BITS-Compact-Server                | N                     |
| BitLocker 磁碟機加密                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Client for NFS                                         | NFS-Client                         | N                     |
| 容器                                             | 容器                         | N                     |
| 資料中心橋接                                   | 資料中心橋               | N                     |
| 增強的存放區                                       | EnhancedStorage                    | N                     |
| 容錯移轉叢集                                    | Failover-Clustering                | N                     |
| 群組原則管理                                | GPMC                               | N                     |
| I/O 服務品質                                 | DiskIo-QoS                         | N                     |
| 可裝載 IIS 的 Web 核心                                  | Web-WHC                            | N                     |
| IP 位址管理 (IPAM) 伺服器                    | IPAM                               | N                     |
| iSNS 伺服器服務                                    | ISNS                               | N                     |
| Management OData IIS 延伸模組                         | ManagementOdata                    | N                     |
| 媒體基礎                                       | Server-Media-Foundation            | N                     |
| 訊息佇列                                        | MSMQ                               | N                     |
| 訊息佇列服務                               | MSMQ-Services                      | N                     |
| 訊息佇列伺服器                                 | MSMQ-Server                        | N                     |
| 目錄服務整合                          | MSMQ-Directory                     | N                     |
| HTTP 支援                                           | MSMQ-HTTP-Support                  | N                     |
| 訊息佇列觸發程序                               | MSMQ 觸發程序                      | N                     |
| 路由服務                                        | MSMQ 路由                       | N                     |
| 訊息佇列 DCOM Proxy                             | MSMQ-DCOM                          | N                     |
| 多重路徑 I/O                                          | 多重路徑 IO                       | N                     |
| MultiPoint 連線程式                                   | MultiPoint-Connector               | N                     |
| MultiPoint 連接器服務                          | MultiPoint-Connector-Services      | N                     |
| MultiPoint 管理員和 MultiPoint 儀表板            | MultiPoint-Tools                   | N                     |
| 網路負載平衡                                 | NLB                                | N                     |
| 對等名稱解析通訊協定                          | PNRP                               | N                     |
| 高品質 Windows 音訊/視訊體驗                 | qWave                              | N                     |
| 遠端差異壓縮                        | RDC                                | N                     |
| 遠端伺服器管理工具                     | RSAT                               | N                     |
| 功能管理工具                           | RSAT-Feature-Tools                 | N                     |
| BitLocker 磁碟機加密管理公用程式  | RSAT-Feature-Tools-BitLocker       | N                     |
| DataCenterBridging LLDP 工具                          | RSAT-DataCenterBridging-LLDP-Tools | N                     |
| 容錯移轉叢集工具                              | RSAT-Clustering                    | N                     |
| 適用於 Windows PowerShell 的容錯移轉叢集模組         | RSAT-Clustering-PowerShell         | N                     |
| 容錯移轉叢集自動化伺服器                     | RSAT-Clustering-AutomationServer   | N                     |
| 容錯移轉叢集命令介面                     | RSAT-Clustering-CmdInterface       | N                     |
| IP 位址管理 (IPAM) 用戶端                    | IPAM-Client-Feature                | N                     |
| 受防護的 VM 工具                                      | RSAT-Shielded-VM-Tools             | N                     |
| 適用於 Windows PowerShell 的存放裝置複本模組          | RSAT-Storage-Replica               | N                     |
| 角色管理工具                              | RSAT-Role-Tools                    | N                     |
| AD DS 與 AD LDS 工具                                 | RSAT-AD-Tools                      | N                     |
| 適用於 Windows PowerShell 的 Active Directory 模組         | RSAT-AD-PowerShell                 | N                     |
| AD DS 工具                                            | RSAT-ADDS                          | N                     |
| Active Directory 管理中心                 | RSAT-AD-AdminCenter                | N                     |
| AD DS 嵌入式管理單元和命令列工具                  | RSAT-ADDS-Tools                    | N                     |
| AD LDS 嵌入式管理單元和命令列工具                 | RSAT-ADLDS                         | N                     |
| Hyper-V 管理工具                               | RSAT-Hyper-V-Tools                 | N                     |
| Windows PowerShell 的 Hyper-V 模組                  | Hyper-V-PowerShell                 | N                     |
| Windows Server Update Services 工具                   | UpdateServices-RSAT                | N                     |
| API 和 PowerShell cmdlet                             | UpdateServices-API                 | N                     |
| DHCP 伺服器工具                                      | RSAT-DHCP                          | N                     |
| DNS 伺服器工具                                       | RSAT-DNS-Server                    | N                     |
| 遠端存取管理工具                         | RSAT-RemoteAccess                  | N                     |
| 適用於 Windows PowerShell 的遠端存取模組            | RSAT-RemoteAccess-PowerShell       | N                     |
| RPC over HTTP Proxy                                    | RPC-over-HTTP-Proxy                | N                     |
| 安裝並啟動事件收集                        | Setup-and-Boot-Event-Collection    | N                     |
| 簡單 TCP/IP 服務                                 | Simple-TCPIP                       | N                     |
| SMB 1.0/CIFS 檔案共用支援                      | FS-SMB1                            | Y                     |
| SMB 頻寬限制                                    | FS-SMBBW                           | N                     |
| SNMP 服務                                           | SNMP-Service                       | N                     |
| SNMP WMI 提供者                                      | SNMP-WMI-Provider                  | N                     |
| Telnet 用戶端                                          | Telnet-Client                      | N                     |
| 適用於網狀架構管理的 VM 防護工具               | FabricShieldedTools                | N                     |
| Windows Defender 功能                              | Windows-Defender-Features          | Y                     |
| Windows Defender                                       | Windows-Defender                   | Y                     |
| Windows 內部資料庫                              | Windows-Internal-Database          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5.1                                 | PowerShell                         | Y                     |
| Windows PowerShell 2.0 Engine                          | PowerShell-V2                      | （移除）             |
| Windows PowerShell Desired State Configuration 服務 | DSC-Service                        | N                     |
| Windows PowerShell Web 存取                          | WindowsPowerShellWebAccess         | N                     |
| Windows 處理序啟用服務                     | WAS                                | N                     |
| 處理序模型                                          | WAS-Process-Model                  | N                     |
| .NET 環境 3.5                                   | WAS-NET-Environment                | N                     |
| 組態API                                     | WAS-Config-APIs                    | N                     |
| Windows Server Backup                                  | Windows-Server-Backup              | N                     |
| Windows Server 移轉工具                         | 移轉                          | N                     |
| Windows 標準式存放裝置管理             | WindowsStorageManagementService    | N                     |
| WinRM IIS 延伸模組                                    | WinRM-IIS-Ext                      | N                     |
| WINS 伺服器                                            | WINS                               | N                     |
| WoW64 支援                                          | WoW64-Support                      | Y                     |
|                                                        |                                    |                       |
