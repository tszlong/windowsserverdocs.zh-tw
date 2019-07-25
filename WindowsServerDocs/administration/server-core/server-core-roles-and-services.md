---
title: Windows Server Core 中包含的角色、角色服務和功能
description: Windows Server 的 Server Core 安裝選項包含哪些角色和功能？
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 2f6aed56083bd606ae2ec06b72152ef4a0461420
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476502"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Windows Server Core 中包含的角色、角色服務和功能

> 適用於：Windows Server 2019、Windows Server 2016 和 Windows Server (半年通道)

我們通常會討論[*不*在 Server Core 中的內容](server-core-removed-roles.md)-現在我們將嘗試不同的方法, 並告訴您所*包含*的內容, 以及是否*預設會安裝*某些專案。 下列角色、角色服務和功能位於 Windows Server 的 Server Core 安裝選項*中*。 使用這項資訊有助於找出伺服器核心選項是否適用于您的環境。 因為這是大型清單, 請考慮搜尋您感興趣的特定角色或功能-如果該搜尋不會傳回您要尋找的內容, 則不會包含在 Server Core 中。

例如, 如果您搜尋「遠端桌面工作階段主機」, 您就不會在此頁面上找到它。 這是因為 RD 工作階段主機不包含在 Server Core 映射中。

請記住, 您[一律可以查看](server-core-removed-roles.md)*未*包含的內容。 這只是查看專案的不同方式。

## <a name="roles-included-in-server-core"></a>Server Core 中包含的角色
[Server Core] 安裝選項包含下列伺服器角色。

| Role                                            | 名稱                           | 預設會安裝嗎？ |
|-------------------------------------------------|--------------------------------|-----------------------|
| Active Directory 憑證服務           | AD-憑證                 | N                     |
| Active Directory Domain Services                | AD-網域服務             | N                     |
| Active Directory 同盟服務            | ADFS-同盟                | N                     |
| Active Directory 輕量型目錄服務 | ADLDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| 裝置健康情況證明                       | DeviceHealthAttestationService | N                     |
| DHCP 伺服器                                     | DHCP                           | N                     |
| DNS 伺服器                                      | DNS                            | N                     |
| 檔案和存放服務                       | FileAndStorage-服務        | Y                     |
| 主機守護者服務                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| 列印和文件服務                     | 列印-服務                 | N                     |
| 遠端存取                                   | RemoteAccess                   | N                     |
| 遠端桌面服務                         | 遠端桌面-服務        | N                     |
| 大量啟用服務                      | Volumeactivation-full-role               | N                     |
| 網頁伺服器 (IIS)                                  | Web 服務器                     | N                     |
| Windows Server Essentials 體驗            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>Server Core 中包含的角色服務
[Server Core] 安裝選項包含下列角色服務。

| Role                                  | 角色服務                                                   | 名稱                    | 預設會安裝嗎？ |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Active Directory 憑證服務 | 憑證授權單位                                        | ADCS-Cert 授權單位     | N                     |
|                                       | 憑證註冊原則 Web 服務                      | ADCS-註冊-Web-Pol     | N                     |
|                                       | 憑證註冊 Web 服務                             | ADCS-註冊-Web-Svc     | N                     |
|                                       | 憑證授權單位網頁註冊。                         | ADCS-Web 註冊     | N                     |
|                                       | 網路裝置註冊服務                              | ADCS-裝置註冊  | N                     |
|                                       | 線上回應                                               | ADCS-線上-Cert        | N                     |
| Active Directory Rights Management    | Active Directory Rights Management Server                      | ADRMS-伺服器            | N                     |
|                                       | 識別身分同盟支援                                    | ADRMS-身分識別          | N                     |
| 檔案和存放服務             | 檔案和 iSCSI 服務                                        | 檔案服務           | N                     |
|                                       | 檔案伺服器                                                    | FS-檔案伺服器           | N                     |
|                                       | 網路檔案的 BranchCache                                  | FS-BranchCache          | N                     |
|                                       | 重複資料刪除                                             | FS-資料重復資料刪除   | N                     |
|                                       | DFS 命名空間                                                 | FS-DFS-命名空間        | N                     |
|                                       | DFS 複寫                                                | FS-DFS-複寫      | N                     |
|                                       | 檔案伺服器資源管理員                                   | FS-資源管理員     | N                     |
|                                       | 檔案伺服器 VSS 代理程式服務                                  | FS-VSS-代理程式            | N                     |
|                                       | iSCSI 目標伺服器                                            | iSCSITarget-伺服器      | N                     |
|                                       | iSCSI 目標儲存提供者 (VDS 和 VSS 硬體提供者) | iSCSITarget-VSS-VDS     | N                     |
|                                       | Server for NFS                                                 | FS-NFS-服務          | N                     |
|                                       | 工作資料夾                                                   | FS-SyncShareService     | N                     |
|                                       | 存放服務                                               | 儲存體-服務        | Y                     |
| 列印和文件服務           | 列印伺服器                                                   | 列印-伺服器            | N                     |
|                                       | LPD 服務                                                    | 列印-LPD-服務       | N                     |
| 遠端存取                         | DirectAccess 和 VPN (RAS)                                     | DirectAccess-VPN        | N                     |
|                                       | 路由                                                        | 路由                 | N                     |
|                                       | Web 應用程式 Proxy                                          | Web 應用程式-Proxy   | N                     |
| 遠端桌面服務               | 遠端桌面連線代理人                               | RDS-連接-代理程式   | N                     |
|                                       | 遠端桌面授權                                       | RDS-授權           | N                     |
|                                       | 遠端桌面虛擬主機                             | RDS-虛擬化      | N                     |
| Web 伺服器 (IIS)                      | Web 伺服器                                                     | Web-web 伺服器           | N                     |
|                                       | 一般 HTTP 功能                                           | Web-通用-Http         | N                     |
|                                       | 預設文件                                               | Web-預設-Doc         | N                     |
|                                       | 瀏覽目錄                                             | Web-Dir-流覽        | N                     |
|                                       | HTTP 錯誤                                                    | Web-Http-錯誤         | N                     |
|                                       | 靜態內容                                                 | Web-靜態-內容      | N                     |
|                                       | HTTP 重新導向                                               | Web-Http-重新導向       | N                     |
|                                       | WebDAV 發行                                              | Web-DAV-發行      | N                     |
|                                       | 狀況及診斷                                         | Web-健全狀況              | N                     |
|                                       | HTTP 記錄                                                   | Web-Http-記錄        | N                     |
|                                       | 自訂記錄                                                 | Web-自訂記錄      | N                     |
|                                       | 記錄工具                                                  | Web-記錄程式庫       | N                     |
|                                       | ODBC 記錄                                                   | Web-ODBC-記錄        | N                     |
|                                       | 要求監視器                                              | Web 要求-監視     | N                     |
|                                       | 追蹤                                                        | Web-Http-追蹤        | N                     |
|                                       | 效能                                                    | Web-效能         | N                     |
|                                       | 靜態內容壓縮                                     | Web-Stat-壓縮    | N                     |
|                                       | 動態內容壓縮                                    | Web-Dyn-壓縮     | N                     |
|                                       | 安全性                                                       | Web-安全性            | N                     |
|                                       | Request Filtering                                              | Web 篩選           | N                     |
|                                       | 基本驗證                                           | Web-基本-驗證          | N                     |
|                                       | 集中式 SSL 憑證支援                            | Web-CertProvider        | N                     |
|                                       | 用戶端憑證對應驗證                      | Web-用戶端-驗證         | N                     |
|                                       | 摘要式驗證                                          | Web-摘要式驗證         | N                     |
|                                       | IIS 用戶端憑證對應驗證                  | Web-Cert-驗證           | N                     |
|                                       | IP 和網域限制                                     | Web-IP-安全性         | N                     |
|                                       | URL 授權                                              | Web-Url-驗證            | N                     |
|                                       | Windows 驗證                                         | Web-Windows-驗證        | N                     |
|                                       | 應用程式開發                                        | Web 應用程式開發             | N                     |
|                                       | .NET 擴充性3。5                                         | 網路-Ext             | N                     |
|                                       | .NET 擴充性4。6                                         | 網路-Ext45           | N                     |
|                                       | 應用程式初始化                                     | Web-Appinit.reg             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3。5                                                    | Web-Asp-Net             | N                     |
|                                       | ASP.NET 4.6                                                    | Web-Asp-Net45           | N                     |
|                                       | CGI                                                            | Web CGI                 | N                     |
|                                       | ISAPI 擴充程式                                               | Web-ISAPI-Ext           | N                     |
|                                       | ISAPI Filters                                                  | Web-ISAPI-篩選器        | N                     |
|                                       | 伺服器端包含                                           | Web-包含            | N                     |
|                                       | WebSocket 通訊協定                                             | Web-Websocket          | N                     |
|                                       | FTP 伺服器                                                     | Web-Ftp-伺服器          | N                     |
|                                       | FTP 服務                                                    | Web-Ftp-服務         | N                     |
|                                       | FTP 擴充性                                              | Web-Ftp-Ext             | N                     |
|                                       | 管理工具                                               | Web-管理工具          | N                     |
|                                       | IIS 6 管理相容性                                 | Web-管理相容性         | N                     |
|                                       | IIS 6 Metabase 相容性                                   | Web-元資料庫            | N                     |
|                                       | IIS 6 腳本工具                                          | Web Lgcy-腳本      | N                     |
|                                       | IIS 6 WMI 相容性                                        | Web-WMI                 | N                     |
|                                       | IIS 管理指令碼及工具                               | Web-腳本-工具     | N                     |
|                                       | 管理服務                                             | Web 管理-服務        | N                     |
| Windows Server Update Services        | WID 連線能力                                               | UpdateServices-WidDB    | N                     |
|                                       | WSUS 服務                                                  | UpdateServices-服務 | N                     |
|                                       | SQL Server 連線能力                                        | UpdateServices-DB       | N                     |

## <a name="features-included-in-server-core"></a>Server Core 中包含的功能
[Server Core] 安裝選項包含下列功能。

| 功能                                                | 名稱                               | 預設會安裝嗎？ |
|--------------------------------------------------------|------------------------------------|-----------------------|
| .NET Framework 3.5 功能                            | NET Framework-功能             | N                     |
| .NET Framework 3.5 (包括 .NET 2.0 和 3.0)       | .NET Framework-核心                 | 拆卸             |
| HTTP 啟動                                        | NET-HTTP-啟用                | N                     |
| 非HTTP 啟動                                    | NET-非 HTTP-作用                 | N                     |
| .NET Framework 4.6 功能                            | NET Framework-45-功能          | Y                     |
| .NET Framework 4.6                                     | NET Framework-45-核心              | Y                     |
| ASP.NET 4.6                                            | NET Framework-45-ASPNET            | N                     |
| WCF 服務                                           | NET-WCF-Services45                 | Y                     |
| HTTP 啟動                                        | NET-WCF-HTTP-Activation45          | N                     |
| 訊息佇列 (MSMQ) 啟用                      | NET-WCF-Activation45          | N                     |
| 具名管道啟用                                  | NET-WCF-管線-Activation45          | N                     |
| TCP 啟用                                         | NET-WCF-TCP-Activation45           | N                     |
| TCP 埠共用                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| 背景智慧型傳送服務 (BITS)         | BITS                               | N                     |
| Compact Server                                         | BITS-Compact-伺服器                | N                     |
| BitLocker 磁碟機加密                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Client for NFS                                         | NFS-用戶端                         | N                     |
| 容器                                             | 容器                         | N                     |
| 資料中心橋接                                   | 資料中心-橋接               | N                     |
| 增強的存放區                                       | EnhancedStorage                    | N                     |
| 容錯移轉叢集                                    | 容錯移轉-叢集                | N                     |
| 群組原則管理                                | GPMC                               | N                     |
| I/O 服務品質                                 | DiskIo-QoS                         | N                     |
| 可裝載 IIS 的 Web 核心                                  | Web-WHC                            | N                     |
| IP 位址管理 (IPAM) 伺服器                    | IPAM                               | N                     |
| iSNS 伺服器服務                                    | ISNS                               | N                     |
| Management OData IIS 延伸模組                         | ManagementOdata                    | N                     |
| 媒體基礎                                       | 伺服器-媒體-基礎            | N                     |
| 訊息佇列                                        | MSMQ                               | N                     |
| 訊息佇列服務                               | MSMQ-服務                      | N                     |
| 訊息佇列伺服器                                 | MSMQ-伺服器                        | N                     |
| 目錄服務整合                          | MSMQ-目錄                     | N                     |
| HTTP 支援                                           | MSMQ-HTTP-支援                  | N                     |
| 訊息佇列觸發程式                               | MSMQ-觸發程式                      | N                     |
| 路由服務                                        | MSMQ-路由                       | N                     |
| 訊息佇列 DCOM Proxy                             | MSMQ-DCOM                          | N                     |
| 多重路徑 I/O                                          | 多重路徑-IO                       | N                     |
| MultiPoint 連線程式                                   | MultiPoint-連接器               | N                     |
| MultiPoint Connector 服務                          | MultiPoint-連接器-服務      | N                     |
| MultiPoint 管理員和 MultiPoint 儀表板            | MultiPoint-工具                   | N                     |
| 網路負載平衡                                 | NLB                                | N                     |
| 對等名稱解析通訊協定                          | PNRP                               | N                     |
| 高品質 Windows 音訊/視訊體驗                 | qWave                              | N                     |
| 遠端差異壓縮                        | RDC                                | N                     |
| 遠端伺服器管理工具                     | RSAT                               | N                     |
| 功能管理工具                           | RSAT-功能工具                 | N                     |
| BitLocker 磁碟機加密系統管理公用程式  | RSAT-功能-工具-BitLocker       | N                     |
| DataCenterBridging LLDP 工具                          | RSAT-DataCenterBridging-LLDP-工具 | N                     |
| 容錯移轉叢集工具                              | RSAT-叢集                    | N                     |
| 適用于 Windows PowerShell 的容錯移轉叢集模組         | RSAT-Clustering-PowerShell         | N                     |
| 容錯移轉叢集自動化伺服器                     | RSAT-叢集-AutomationServer   | N                     |
| 容錯移轉叢集命令介面                     | RSAT-叢集-CmdInterface       | N                     |
| IP 位址管理 (IPAM) 用戶端                    | IPAM-用戶端功能                | N                     |
| 受防護的 VM 工具                                      | RSAT-受防護-VM-工具             | N                     |
| 適用于 Windows PowerShell 的儲存體複本模組          | RSAT-儲存體-複本               | N                     |
| 角色管理工具                              | RSAT-角色-工具                    | N                     |
| AD DS 和 AD LDS 工具                                 | RSAT-AD-工具                      | N                     |
| 適用於 Windows PowerShell 的 Active Directory 模組         | RSAT-AD-PowerShell                 | N                     |
| AD DS 工具                                            | RSAT-新增                          | N                     |
| Active Directory 管理中心                 | RSAT-AD-AdminCenter                | N                     |
| AD DS 嵌入式管理單元和命令列工具                  | RSAT-新增-工具                    | N                     |
| AD LDS 嵌入式管理單元和命令列工具                 | RSAT-ADLDS                         | N                     |
| Hyper-V 管理工具                               | RSAT-Hyper-v-工具                 | N                     |
| Windows PowerShell 的 Hyper-V 模組                  | Hyper-V-PowerShell                 | N                     |
| Windows Server Update Services 工具                   | UpdateServices-RSAT                | N                     |
| API 和 PowerShell Cmdlet                             | UpdateServices-API                 | N                     |
| DHCP 伺服器工具                                      | RSAT-DHCP                          | N                     |
| DNS 伺服器工具                                       | RSAT-DNS-伺服器                    | N                     |
| 遠端存取管理工具                         | RSAT-RemoteAccess                  | N                     |
| 適用於 Windows PowerShell 的遠端存取模組            | RSAT-RemoteAccess-PowerShell       | N                     |
| RPC over HTTP Proxy                                    | RPC-over HTTP-Proxy                | N                     |
| 安裝並啟動事件收集                        | 設定與開機-事件收集    | N                     |
| 簡單 TCP/IP 服務                                 | 簡單-TCPIP                       | N                     |
| SMB 1.0/CIFS 檔案共用支援                      | FS-SMB1                            | Y                     |
| SMB 頻寬限制                                    | FS-SMBBW                           | N                     |
| SNMP 服務                                           | SNMP-服務                       | N                     |
| SNMP WMI 提供者                                      | SNMP-WMI-提供者                  | N                     |
| Telnet 用戶端                                          | Telnet 用戶端                      | N                     |
| 適用於網狀架構管理的 VM 防護工具               | FabricShieldedTools                | N                     |
| Windows Defender 功能                              | Windows-Defender-功能          | Y                     |
| Windows Defender                                       | Windows-Defender                   | Y                     |
| Windows 內部資料庫                              | Windows-內部-資料庫          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5。1                                 | PowerShell                         | Y                     |
| Windows PowerShell 2.0 引擎                          | PowerShell-V2                      | 拆卸             |
| Windows PowerShell Desired State Configuration 服務 | DSC-服務                        | N                     |
| Windows PowerShell Web 存取                          | WindowsPowerShellWebAccess         | N                     |
| Windows 處理序啟用服務                     | 為                                | N                     |
| 處理序模型                                          | WAS-進程模型                  | N                     |
| .NET 環境3。5                                   | WAS-NET-環境                | N                     |
| 組態API                                     | WAS-Config-Api                    | N                     |
| Windows Server Backup                                  | Windows-伺服器-備份              | N                     |
| Windows Server 移轉工具                         | 移轉                          | N                     |
| Windows 標準式存放裝置管理             | WindowsStorageManagementService    | N                     |
| WinRM IIS 延伸模組                                    | WinRM-IIS-Ext                      | N                     |
| WINS 伺服器                                            | WINS                               | N                     |
| WoW64 支援                                          | WoW64-支援                      | Y                     |
|                                                        |                                    |                       |
