---
title: Windows Server 2019 Standard 和 Datacenter 版本的比較
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
ms.assetid: c5ca3bfe-7ced-49f6-2932-80cab33fe914
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 2cb368b5ff4723c1afb53c5a9e787d9f8a39bef3
ms.sourcegitcommit: 40e4ba214954d198936341c4d6ce1916dc891169
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690452"
---
# <a name="comparison-of-standard-and-datacenter-editions-of-windows-server-2019"></a>Windows Server 2019 Standard 和 Datacenter 版本的比較

> 適用於：Windows Server 2019
  
## <a name="locks-and-limits"></a>鎖定與限制

| 鎖定與限制                 | Windows Server 2019 Standard | Windows Server 2019 Datacenter |  
| -------------------              | ---------------------------  | --------------------------- |  
| 使用者數目上限          | 以 CAL 為依據                | 以 CAL 為依據 |
| SMB 連線數上限          | 16,777,216                   | 16,777,216 |
| RRAS 連線數上限         | 沒有限制                    | 沒有限制 |
| IAS 連線數上限          | 2,147,483,647                | 2,147,483,647 |
| RDS 連線數上限          | 65,535                       | 65,535 |
| 64 位元通訊端數目上限 | 64                           | 64 |
| 核心數目上限          | 沒有限制                    | 沒有限制 |
| RAM 上限                      | 24 TB                        | 24 TB |
| 可以用作虛擬客體 | 是，2 部虛擬機器，加上每份授權一部 Hyper-V 主機|是，<strong>無限制的虛擬機器</strong>，加上每份授權一部 Hyper-V 主機 |
| 伺服器可加入網域        | 是                           | 是 |
| 邊緣網路保護/防火牆| 否                            | 否    |
| DirectAccess                    | 是                           | 是 |
| DLNA 轉碼器和網路媒體串流 | 是，若以桌面體驗安裝為伺服器 | 是，若以桌面體驗安裝為伺服器 |

## <a name="server-roles"></a>伺服器角色

|可用的 Windows Server 角色|角色服務|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|----------|---------------------------|  
|Active Directory 憑證服務| |是|是|
|Active Directory Domain Services| |是|是|
|Active Directory 同盟服務| |是|是|
|AD 輕量型目錄服務| |是|是|
|AD Rights Management Services| |是|是|
|裝置健康情況證明| |是|是|
|DHCP 伺服器| |是|是|
|DNS 伺服器| |是|是|
|傳真伺服器| |是|是|
|檔案和存放服務|檔案伺服器|是|是|
|檔案和存放服務|網路檔案的 BranchCache|是|是|
|檔案和存放服務|重複資料刪除|是|是|
|檔案和存放服務|DFS 命名空間|是|是|
|檔案和存放服務|DFS 複寫|是|是|
|檔案和存放服務|檔案伺服器資源管理員|是|是|
|檔案和存放服務|檔案伺服器 VSS 代理程式服務|是|是|
|檔案和存放服務|iSCSI 目標伺服器|是|是|
|檔案和存放服務|iSCSI 目標儲存提供者|是|是|
|檔案和存放服務|Server for NFS|是|是|
|檔案和存放服務|工作資料夾|是|是|
|檔案和存放服務|存放服務|是|是|
|主機守護者服務| |是|是|
|Hyper-V| |是|是；包含受防護的虛擬機器|
|網路控制卡| |否| <strong>是</strong> |
|網路原則與存取服務| |是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|列印和文件服務| |是|是|
|遠端存取| |是|是|
|遠端桌面服務| |是|是|
|大量啟用服務| |是|是|
|Web 服務 (IIS)| |是|是|
|Windows 部署服務| |是*|是*|
|Windows Server Essentials 體驗| |否 | 否 |
|Windows Server Update Services| |是|是|

*WDS 傳輸伺服器是 Windows Server 2019 中 Server Core 安裝的新功能 (也將出現在從 Windows Server 1803 版開始的半年通道)


## <a name="features"></a>功能

|可透過伺服器管理員 (或 PowerShell) 安裝的 Windows Server 功能|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|---------------------------|  
|.NET Framework 3.5 |是|是|
|.NET Framework 4.7 |是|是|
|背景智慧型傳送服務 (BITS)|是|是|
|BitLocker 磁碟機加密|是|是|
|BitLocker 網路解除鎖定|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|BranchCache|是|是|
|Client for NFS|是|是|
|容器|是 (無限制的 Windows 容器；最多 2 個 Hyper-V 容器)|是 (<strong>無限制的 Windows 和 Hyper-V 容器</strong>) |
|資料中心橋接|是|是|
|Direct Play|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|增強的存放區|是|是|
|容錯移轉叢集|是|是|
|群組原則管理|是|是|
|主機守護者 Hyper-V 支援|否| <strong>是</strong> |
|I/O 服務品質|是|是|
|可裝載 IIS 的 Web 核心|是|是|
|網際網路列印用戶端|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|IPAM 伺服器|是|是|
|iSNS 伺服器服務|是|是|
|LPR 連接埠監視器|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|Management OData IIS 延伸模組|是|是|
|媒體基礎|是|是|
|訊息佇列|是|是|
|多重路徑 I/O|是|是|
|MultiPoint 連線程式|是|是|
|網路負載平衡|是|是|
|對等名稱解析通訊協定|是|是|
|高品質 Windows 音訊/視訊體驗|是|是|
|RAS 連線管理員系統管理組件|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|遠端協助|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|遠端差異壓縮|是|是|
|RSAT|是|是|
|RPC over HTTP Proxy|是|是|
|安裝並啟動事件收集|是|是|
|簡單 TCP/IP 服務|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|SMB 1.0/CIFS 檔案共用支援|已安裝|已安裝|
|SMB 頻寬限制|是|是|
|SMTP 伺服器|是|是|
|SNMP 服務|是|是|
|軟體負載平衡器|是|是|
|儲存體複本|是|是|
|Telnet 用戶端|是|是|
|TFTP 用戶端|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|適用於網狀架構管理的 VM 防護工具|是|是|
|WebDAV 重新導向器|是|是|
|Windows 生物特徵辨識架構|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|Windows Defender 功能|已安裝|已安裝|
|Windows Identity Foundation 3.5|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|Windows 內部資料庫|是|是|
|Windows PowerShell|已安裝|已安裝|
|Windows 處理序啟用服務|是|是|
|Windows Search 服務|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|Windows Server Backup|是|是|
|Windows Server 移轉工具|是|是|
|Windows 標準式存放裝置管理|是|是|
|Windows TIFF IFilter|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|WinRM IIS 延伸模組|是|是|
|WINS 伺服器|是|是|
|無線區域網路服務|是|是|
|WoW64 支援|已安裝|已安裝|
|XPS 檢視器|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|

|正式運作的功能|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|---------------------------|  
|最佳做法分析程式|是|是|
|直接存取|是|是|
|動態記憶體 (虛擬環境中)|是|是|
|熱新增/取代 RAM|是|是|
|Microsoft Management Console|是|是|
|基本伺服器介面|是|是|
|網路負載平衡|是|是|
|Windows PowerShell|是|是|
|Server Core 安裝選項|是|是|
|伺服器管理員|是|是|
|SMB 直接傳輸與 SMB (透過 RDMA)|是|是|
|軟體定義網路|否| <strong>是</strong> |
|儲存體移轉服務|是|是|
| 儲存體複本         | 是 (1 個合作關係及具有單一 2TB 磁碟區的 1 個資源群組)    | 是，<strong>無限制</strong> |
|儲存空間|是|是|
|儲存空間直接存取|否| <strong>是</strong> |
|大量啟用服務|是|是|
|VSS (磁碟區陰影複製服務) 整合|是|是|
|Windows Server Update Services|是|是|
|Windows 系統資源管理員|是|是|
|伺服器授權記錄|是|是|
|已繼承的啟用|若託管於 Datacenter 則作為客體| <strong>可以是主機或來賓</strong> |
|工作資料夾|是|是|
