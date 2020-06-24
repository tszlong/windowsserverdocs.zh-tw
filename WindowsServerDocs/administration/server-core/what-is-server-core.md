---
title: 什麼是 Server Core？
description: 瞭解 Windows Server 中的 Server Core 安裝選項
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 17bca691fef0ed9478c8ddb49e0511b0a16ac7b7
ms.sourcegitcommit: 75e87fef264e30af3dfeb57923d5d82b0c51de5d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85279613"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>Windows Server 中的 Server Core 安裝選項是什麼？

> 適用于： Windows Server 2019、Windows Server 2016 和 Windows Server （半年通道）

當您部署 Standard 或 Datacenter edition 的 Windows Server 時，[Server Core] 選項是最小的安裝選項。 Server Core 包含大部分但並非全部的伺服器角色。 伺服器核心的磁片使用量較小，因此是較小的攻擊面，因為程式碼基底較小。

## <a name="server-core-vs-server-with-desktop-experience"></a>具有桌面體驗的伺服器（核心） vs Server

當您安裝 Windows Server 時，只會安裝所選的伺服器角色，這有助於降低 Windows Server 的整體使用量。 不過，[具有桌面體驗的伺服器] 安裝選項仍然會安裝許多服務，以及特定使用案例通常不需要的其他元件。

這就是伺服器核心開始的地方： Server Core 安裝會排除對某些常用伺服器角色的支援不重要的任何服務和其他功能。 例如，Hyper-v 伺服器不需要圖形化使用者介面（GUI），因為您可以從命令列使用 Windows PowerShell 或使用 Hyper-v 管理員從遠端系統管理 Hyper-v 的所有層面。

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>Server Core 差異-不含樸素的核心功能

當您完成在系統上安裝 Server Core 並第一次登入時，您會感到驚訝。 [使用桌面體驗的伺服器] 安裝選項和 [Server Core] 的主要差異在於 Server Core 不包含下列 GUI shell 套件：

- Microsoft-Windows-伺服器-Shell-封裝
- Microsoft-Windows-伺服器-Gui-管理-套件
- Microsoft-Windows-Server-Gui-RSAT-封裝
- Microsoft-Windows-Cortana-PAL-桌上型-套件

換句話說，伺服器核心中的設計並**沒有桌面**。 雖然維護支援傳統商務應用程式和以角色為基礎的工作負載所需的功能，但 Server Core 並沒有傳統的桌面介面。 相反地，伺服器核心是設計為透過命令列、PowerShell 或 GUI 工具（例如[RSAT](../../remote/remote-server-administration-tools.md)或[Windows 系統管理中心](../../manage/windows-admin-center/overview.md)）從遠端系統管理。

除了沒有 UI 以外，伺服器核心也會以下列方式與具有桌面體驗的伺服器不同：

- Server Core 沒有任何協助工具工具
- 設定 Server Core 沒有 OOBE （全新體驗）
- 不支援音訊

下表顯示哪些應用程式可在具有桌面體驗的 Server Core vs Server*本機*上使用。 **重要**事項：在大部分情況下，列為「無法使用」的應用程式可以從 Windows 用戶端電腦遠端執行，並用來管理您的 Server Core 安裝。

> [!NOTE]
> 這份清單的目的是要提供快速參考，這不是完整的清單。


| Application                        | Server Core     | 具備桌面體驗的伺服器 |
|------------------------------------|-----------------|--------------------------------|
| 命令提示字元                     | 可供使用       | 可供使用                      |
| Windows PowerShell/Microsoft .NET | 可供使用       | 可供使用                      |
| Perfmon.exe                        | 無法使用   | 可供使用                      |
| Windbg （GUI）                       | 支援       | 支援                      |
| Resmon.exe                         | 無法使用   | 可供使用                      |
| Regedit                            | 可供使用       | 可供使用                      |
| Fsutil.exe                         | 可供使用       | 可供使用                      |
| Disksnapshot.exe                   | 無法使用   | 可供使用                      |
| Diskpart.exe                       | 可供使用       | 可供使用                      |
| Diskmgmt.msc services.msc                       | 無法使用   | 可供使用                      |
| Devmgmt.msc services.msc                        | 無法使用   | 可供使用                      |
| 伺服器管理員                     | 無法使用   | 可供使用                      |
| Mmc.exe                            | 無法使用   | 可供使用                      |
| Eventvwr.msc                           | 無法使用   | 可供使用                      |
| Wevtutil （事件查詢）           | 可供使用       | 可供使用                      |
| Services.msc                       | 無法使用   | 可供使用                      |
| 控制台                      | 無法使用   | 可供使用                      |
| Windows Update （GUI）               | 無法使用   | 可供使用                      |
| Windows 檔案總管                   | 無法使用   | 可供使用                      |
| 工作列                            | 無法使用   | 可供使用                      |
| 工作列通知              | 無法使用   | 可供使用                      |
| Taskmgr                            | 可供使用       | 可供使用                      |
| Internet Explorer 或 Edge          | 無法使用   | 可供使用                      |
| 內建說明系統               | 無法使用   | 可供使用                      |
| Windows 10 Shell                   | 無法使用   | 可供使用                      |
| Windows Media Player               | 無法使用   | 可供使用                      |
| PowerShell                         | 可供使用       | 可供使用                      |
| PowerShell ISE                     | 無法使用   | 可供使用                      |
| PowerShell 輸入法                     | 可供使用       | 可供使用                      |
| Mstsc.exe                          | 無法使用   | 可供使用                      |
| 遠端桌面服務            | 可供使用       | 可供使用                      |
| Hyper-V 管理員                    | 無法使用   | 可供使用                      |
| WordPad\*                          | 無法使用   | 可供使用                      |


如需 Server Core 內含*內容的*詳細資訊，請參閱[角色、角色服務和 Windows server core 中包含的功能](server-core-roles-and-services.md)。 如需 Server Core 中*未*包含內容的相關資訊，請參閱[不包含在 server Core 中的角色、角色服務和功能](server-core-removed-roles.md)。

\*要讀取的。RTF 檔案儲存在伺服器核心 SKU 上，使用者可以將檔案複製到 WordPad 所在的另一部 Windows 電腦上。

## <a name="get-started-using-server-core"></a>開始使用 Server Core

使用下列資訊來安裝、設定和管理 Windows Server 的 Server Core 安裝選項。

Server Core 安裝：
- [Server Core 中包含的角色、角色服務和功能](server-core-roles-and-services.md)
- [不在 Server Core 中的角色、角色服務和功能](server-core-removed-roles.md)
- [安裝 Server Core 安裝選項](../../get-started/getting-started-with-server-core.md)
- [使用 SConfig 工具設定 Server Core](../../get-started/sconfig-on-ws2016.md)

使用 Server Core：
- [使用 Windows PowerShell 或命令列的基本伺服器核心管理工作](server-core-administer.md)
- [管理 Server Core](server-core-manage.md)
- [修補伺服器核心](server-core-servicing.md)
- [設定記憶體傾印檔案](server-core-memory-dump.md)
