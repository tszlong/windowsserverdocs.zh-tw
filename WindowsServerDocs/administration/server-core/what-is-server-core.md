---
title: 什麼是 Server Core？
description: 深入了解 Windows Server 中的 Server Core 安裝選項
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 08229e458d0aa0c8e8397f0f053f37a207a1aea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885599"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>什麼是 Windows Server 中的 Server Core 安裝選項？

> 適用於：Windows Server （半年通道） 和 Windows Server 2016

Server Core 選項是當您部署的 Windows Server Standard 或 Datacenter edition 是可用的最小安裝選項。 Server Core 包含大部分但並非所有的伺服器角色。 Server Core 具有較小的磁碟使用量，因此較小的受攻擊面，因為較小的程式碼基底。 

## <a name="server-core-vs-server-with-desktop-experience"></a>伺服器 （核心） 與伺服器含桌面體驗 
當您安裝 Windows Server 時，您會安裝僅伺服器角色，您選擇-這有助於減少適用於 Windows Server 的整體使用量。 不過，「 Server 含桌面體驗安裝選項仍會安裝許多服務和特定使用案例通常不需要其他元件。 

這是在 Server Core 派上用場： 的 Server Core 安裝可消除所有的服務和其他功能的不必要的特定支援常使用的伺服器角色。 例如，HYPER-V 伺服器不需要圖形化使用者介面 (GUI)，因為您可以從命令列使用 Windows PowerShell，或使用 HYPER-V 管理員從遠端管理 HYPER-V 的各個層面。 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>Server Core 的差異-核心功能，不必 frills
當您完成 Server Core 上安裝系統及登入第一次時，您會處於感到驚訝的位元。 「 Server 含桌面體驗安裝選項和 Server Core 的主要差異是 Server Core 不包含下列的 GUI 殼層套件：

- Microsoft-Windows-Server-Shell-Package
- Microsoft-Windows-Server-Gui-Mgmt-Package
- Microsoft-Windows-Server-Gui-RSAT-Package
- Microsoft-Windows-Cortana-PAL-Desktop-Package

換句話說，沒有**任何桌面**在 Server Core 中，所設計。 同時支援傳統商務應用程式和以角色為基礎的工作負載所需的功能，Server Core 沒有傳統的桌面介面。 相反地，Server Core 設計來透過命令列、 PowerShell 或 GUI 工具從遠端管理 (例如[RSAT](../../remote/remote-server-administration-tools.md)或是[Windows Admin Center](../../manage/windows-admin-center/overview.md))。

除了沒有 UI，Server Core 也不同於伺服器含桌面體驗功能如下：

- Server Core 沒有任何協助工具
- 沒有 OOBE （out-的--體驗） 來設定 Server Core
- 沒有音訊支援

下表顯示哪些應用程式的可用性*本機*在 Server Core 或伺服器含桌面體驗。 **重要**:在大部分的情況下，會列為 「 無法使用 」 下方可以從遠端執行從 Windows 用戶端電腦，以及用來管理 Server Core 安裝的應用程式。

> [!NOTE]
> 這份清單適用於快速參考-它不是完整的清單。


| 應用程式                     | Server Core     | 具備桌面體驗的伺服器 |
|------------------------------------|-----------------|--------------------------------|
| 命令提示字元                     | 線上 (available)       | 線上 (available)                      |
| Windows PowerShell/ Microsoft .NET | 線上 (available)       | 線上 (available)                      |
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
| Wevtutil （事件的查詢）           | 線上 (available)       | 線上 (available)                      |
| Services.msc                       | 無法使用   | 線上 (available)                      |
| 控制台                      | 無法使用   | 線上 (available)                      |
| Windows Update (GUI)                 | 無法使用 | 線上 (available)                      |
| Windows 檔案總管                   | 無法使用   | 線上 (available)                      |
| 工作列                            | 無法使用   | 線上 (available)                      |
| 工作列通知              | 無法使用   | 線上 (available)                      |
| taskmgr                            | 線上 (available)       | 線上 (available)                      |
| Internet Explorer 或 Edge          | 無法使用   | 線上 (available)                      |
| 內建說明系統               | 無法使用   | 線上 (available)                      |
| Windows 10 Shell                   | 無法使用   | 線上 (available)                      |
| Windows Media Player               | 無法使用   | 線上 (available)                      |
| PowerShell                         | 線上 (available)       | 線上 (available)                      |
| PowerShell ISE                     | 無法使用   | 線上 (available)                      |
| PowerShell IME                     | 線上 (available)       | 線上 (available)                      |
| Mstsc.exe                          | 無法使用   | 線上 (available)                      |
| 遠端桌面服務            | 線上 (available)       | 線上 (available)                      |
| Hyper-V 管理員                    | 無法使用  | 線上 (available)                      |


如需詳細資訊，該怎麼辦*已*包含在 Server Core，請參閱[角色、 角色服務和功能包含在 Windows Server 的 Server Core](server-core-roles-and-services.md)。 和的相關資訊*不是*包含在 Server Core，請參閱[角色、 角色服務和功能不包含在 Server Core](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>開始使用 Server Core
您可以使用下列資訊來安裝、 設定和管理 Windows Server 的 Server Core 安裝選項。

Server Core 安裝： 
- [角色、 角色服務和功能包含在 Server Core](server-core-roles-and-services.md)
- [角色、 角色服務和功能不在 Server Core](server-core-removed-roles.md)
- [安裝 Server Core 安裝選項](../../get-started/getting-started-with-server-core.md)
- [設定 Server Core 使用 SConfig 工具](../../get-started/sconfig-on-ws2016.md)

使用 Server Core:
- [基本的 Server Core 系統管理工作，使用 Windows PowerShell 或命令列](server-core-administer.md)
- [管理 Server Core](server-core-manage.md)
- [修補的伺服器核心](server-core-servicing.md)
- [設定記憶體傾印檔案](server-core-memory-dump.md)