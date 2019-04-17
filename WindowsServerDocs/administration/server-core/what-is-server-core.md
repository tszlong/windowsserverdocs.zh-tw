---
title: 什麼是 Server Core？
description: 了解在 Windows Server 中的伺服器核心安裝選項
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 08229e458d0aa0c8e8397f0f053f37a207a1aea5
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1718532"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>什麼是 Windows Server 中的伺服器核心安裝選項？

> 適用於： Windows Server （分號每年次通道） 和 Windows Server 2016

[伺服器核心] 選項是當您要部署 Standard 或 Datacenter 版本的 Windows Server 的基本安裝選項。 伺服器核心包含大部分但不是所有伺服器角色。 伺服器核心具有較小的磁碟使用量，與因此因較小的程式碼基底而較小的攻擊層面。 

## <a name="server-core-vs-server-with-desktop-experience"></a>伺服器 （核心） 與伺服器與桌面體驗 
當您安裝 Windows Server 時，會安裝僅伺服器角色的您選擇-這有助於降低整體之上的 Windows Server。 不過，伺服器與桌面體驗安裝選項仍安裝許多服務和其他通常不需要特定的流量分析藍本的元件。 

這是伺服器核心會進入播放： Server Core 安裝排除任何服務及其他功能不支援某些基本常使用的伺服器角色。 例如，因為其中一個從命令列使用 Windows PowerShell 或使用 HYPER-V 管理員從遠端管理虛擬的方式各方面的 HYPER-V HYPER-V 伺服器不需要圖形化使用者介面 (GUI)。 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>伺服器核心差異-沒有 frills 核心功能
當您完成在系統和登入的伺服器核心安裝第一次時，您正處於意外的位元的。 伺服器與桌面體驗安裝選項和伺服器核心的主要差異在於伺服器核心不包括下列 GUI 介面套件：

- Microsoft Windows-伺服器-介面-套件
- Microsoft-Windows-Server-Gui-Mgmt-Package
- Microsoft-Windows-Server-Gui-RSAT-Package
- Microsoft-Windows-Cortana-PAL-Desktop-Package

換句話說中, 有**任何桌面**Server Core 所設計。 同時又不支援傳統商務應用程式與角色為基礎的工作負載所需的功能、 伺服器核心沒有傳統桌面介面。 而是伺服器核心的設計透過命令列、 PowerShell 中或 （例如[RSAT](../../remote/remote-server-administration-tools.md)或[Windows 系統管理中心](../../manage/windows-admin-center/overview.md)） GUI 工具從遠端管理。

除了沒有 UI 伺服器核心也不同於伺服器與桌面體驗方式如下：

- 伺服器核心並沒有任何協助工具
- 伺服器核心安裝的設定沒有 OOBE （（英文）--方塊-經驗）
- 無音訊的支援

下表顯示哪些應用程式是可*在本機*伺服器核心 vs 與桌面體驗的伺服器上。 **重要**： 在大多數情況下所列示為 「 無法使用 「 下方可以從遠端執行從 Windows 用戶端電腦與用來管理您的伺服器核心安裝應用程式。

> [!NOTE]
> 這份清單適合的快速參考 （英文）-它不適用對象為完整清單。


| 應用程式                     | Server Core     | 具備桌面體驗的伺服器 |
|------------------------------------|-----------------|--------------------------------|
| 命令提示字元                     | 線上 (available)       | 線上 (available)                      |
| Windows PowerShell / Microsoft.NET | 線上 (available)       | 線上 (available)                      |
| Perfmon.exe                        | 無法使用  | 線上 (available)                      |
| Windbg (GUI)                         | 支援       | 支援                      |
| Resmon.exe                         | 無法使用   | 線上 (available)                      |
| Regedit                            | 線上 (available)       | 線上 (available)                      |
| Fsutil.exe                         | 線上 (available)       | 線上 (available)                      |
| Disksnapshot.exe                   | 無法使用   | 線上 (available)                      |
| Diskpart.exe                       | 線上 (available)       | 線上 (available)                      |
| Diskmgmt.msc                       | 無法使用   | 線上 (available)                      |
| Devmgmt.msc                        | 無法使用   | 線上 (available)                      |
| 伺服器管理員                     | 無法使用  | 線上 (available)                      |
| Mmc.exe                            | 無法使用   | 線上 (available)                      |
| Eventvwr                           | 無法使用  | 線上 (available)                      |
| Wevtutil （事件查詢）           | 線上 (available)       | 線上 (available)                      |
| Services.msc                       | 無法使用   | 線上 (available)                      |
| 控制台                      | 無法使用   | 線上 (available)                      |
| Windows Update (GUI)                 | 無法使用 | 線上 (available)                      |
| Windows 檔案總管                   | 無法使用   | 線上 (available)                      |
| 工作列                            | 無法使用   | 線上 (available)                      |
| 工作列的通知              | 無法使用   | 線上 (available)                      |
| Taskmgr                            | 線上 (available)       | 線上 (available)                      |
| Internet Explorer 或 Edge          | 無法使用   | 線上 (available)                      |
| 內建說明系統               | 無法使用   | 線上 (available)                      |
| Windows 10 命令介面                   | 無法使用   | 線上 (available)                      |
| Windows Media Player               | 無法使用   | 線上 (available)                      |
| PowerShell                         | 線上 (available)       | 線上 (available)                      |
| PowerShell ISE                     | 無法使用   | 線上 (available)                      |
| PowerShell 輸入法                     | 線上 (available)       | 線上 (available)                      |
| Mstsc.exe                          | 無法使用   | 線上 (available)                      |
| 遠端桌面服務            | 線上 (available)       | 線上 (available)                      |
| Hyper-V 管理員                    | 無法使用  | 線上 (available)                      |


如需哪些*已*包含在 Server Core 的詳細資訊，請參閱[角色、 角色服務和 Windows Server-Server Core 中包含的功能](server-core-roles-and-services.md)。 如需為何*不*包含在 Server Core 資訊，請參閱[角色、 角色服務和功能不包含在 Server Core](server-core-removed-roles.md)和

## <a name="get-started-using-server-core"></a>開始使用伺服器核心
使用下列資訊可安裝、 設定及管理 Windows Server 的伺服器核心安裝選項。

伺服器核心安裝： 
- [角色、 角色服務和伺服器核心中包含的功能](server-core-roles-and-services.md)
- [角色、 角色服務和功能不在 Server Core](server-core-removed-roles.md)
- [安裝的伺服器核心安裝選項](../../get-started/getting-started-with-server-core.md)
- [使用 SConfig 工具設定伺服器核心](../../get-started/sconfig-on-ws2016.md)

使用 Server Core：
- [使用 Windows PowerShell 或命令列的基本伺服器核心管理工作](server-core-administer.md)
- [管理伺服器核心](server-core-manage.md)
- [修補伺服器核心](server-core-servicing.md)
- [設定記憶體傾印檔案](server-core-memory-dump.md)