---
title: Windows Server 2019 Standard 和 Datacenter 版本的比較
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5ca3bfe-7ced-49f6-2932-80cab33fe914
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: e94c88ab7fb4aede7554dd621843b613fa16b6dc
ms.sourcegitcommit: 0b51cf8d0a1db31d6d2e2e45a145011311fbdf34
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2019
ms.locfileid: "9169932"
---
# Windows Server 2019 Standard 和 Datacenter 版本的比較

> 適用於︰Windows Server 2019
  
## 鎖定與限制
|鎖定與限制|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|---------------------------|  
|使用者數目上限|以 CAL 為依據|以 CAL 為依據|
|SMB 連線數上限|16777216|16777216|
|RRAS 連線數上限|無限制|無限制|
|IAS 連線數上限|2147483647|2147483647|
|RDS 連線數上限|65535|65535|
|64 位元通訊端數目上限|64|64|
|核心數目上限|無限制|無限制|
|RAM 上限|24 TB|24 TB|
|可以用作虛擬客體|是，2 部虛擬機器，加上每份授權一部 Hyper-V 主機|是，無限制的虛擬機器，加上每份授權一部 Hyper-V 主機|
|伺服器可加入網域|是|是|
|邊緣網路保護/防火牆|否|否|
|DirectAccess|是|是|
|DLNA 轉碼器和網路媒體串流|是，若以桌面體驗安裝為伺服器|是，若以桌面體驗安裝為伺服器|

## 伺服器角色
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
|網路控制卡| |否|是|
|網路原則與存取服務| |是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|列印和文件服務| |是|是|
|遠端存取| |是|是|
|遠端桌面服務| |是|是|
|大量啟用服務| |是|是|
|Web 服務 (IIS)| |是|是|
|Windows 部署服務| |是*|是*|
|WindowsServer Essentials 體驗| |是|是|
|WindowsServer Update Services| |是|是|

* WDS 傳輸伺服器是 Windows Server 2019 中的 Server Core 安裝 （也在從 Windows Server 版本 1803年開始半年度管道） 的新功能


## 功能

|可透過伺服器管理員 (或 PowerShell) 安裝的 Windows Server 功能|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|---------------------------|  
|.NET Framework 3.5|是|是|
|.NET Framework 4.6|是|是|
|背景智慧型傳送服務 (BITS)|是|是|
|BitLocker 磁碟機加密|是|是|
|BitLocker 網路解除鎖定|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|BranchCache|是|是|
|Client for NFS|是|是|
|容器|是 (Windows 容器無限制；Hyper-V 容器最多 2 個)|是（所有容器類型無限制）|
|資料中心橋接|是|是|
|Direct Play|是，以桌面體驗安裝為伺服器時|是，以桌面體驗安裝為伺服器時|
|增強的存放區|是|是|
|容錯移轉叢集|是|是|
|群組原則管理|是|是|
|主機守護者 Hyper-V 支援|否|是|
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
|儲存體複本|否|是|
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
|限制式的儲存體複本|是的 （1 個合作關係和 1 資源群組使用單一 2 TB 的磁碟區）|\ [是，無限制|
|直接存取|是|是|
|動態記憶體 (虛擬環境中)|是|是|
|熱新增/取代 RAM|是|是|
|Microsoft Management Console|是|是|
|基本伺服器介面|是|是|
|網路負載平衡|是|是|
|Windows PowerShell|是|是|
|Server Core 安裝選項|是|是|
|Nano Server 安裝選項|是|是|
|伺服器管理員|是|是|
|SMB 直接傳輸與 SMB (透過 RDMA)|是|是|
|軟體定義的網路|否|是|
|存放管理服務|是|是|
|儲存空間|是|是|
|儲存空間直接存取|否|是|
|大量啟用服務|是|是|
|VSS (磁碟區陰影複製服務) 整合|是|是|
|WindowsServer Update Services|是|是|
|Windows 系統資源管理員|是|是|
|伺服器授權記錄|是|是|
|已繼承的啟用|若託管於 Datacenter 則作為客體|可為主機或客體|
|工作資料夾|是|是|

