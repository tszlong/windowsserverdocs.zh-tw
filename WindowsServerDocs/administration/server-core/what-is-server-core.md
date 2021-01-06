---
title: 什麼是 Server Core？
description: 瞭解 Windows Server 中的 Server Core 安裝選項
ms.mktglfcycl: manage
ms.sitesec: library
author: pronichkin
ms.author: artemp
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.topic: conceptual
ms.openlocfilehash: 6bfcd16ed8e4c834cfb32eea4774eb08da2f12b4
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97943624"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>什麼是 Windows Server 中的 Server Core 安裝選項？

> 適用于： Windows Server 2019、Windows Server 2016 和 Windows Server (半年通道) 

當您部署 Standard 或 Datacenter edition 的 Windows Server 時，Server Core 選項是最基本的安裝選項。 Server Core 包含大部分但並非所有的伺服器角色。 因為較小的程式碼基底，所以 Server Core 的磁片使用量較小，因此是較小的受攻擊面。

## <a name="server-core-vs-server-with-desktop-experience"></a>Server (Core) vs Server 含桌面體驗

當您安裝 Windows Server 時，只會安裝您所選擇的伺服器角色，這有助於降低 Windows Server 的整體使用量。 不過，[具有桌面體驗的伺服器] 安裝選項仍會安裝許多服務以及特定使用案例中通常不需要的其他元件。

這就是伺服器核心的播放位置： Server Core 安裝會排除任何不必要的服務和其他功能，以支援某些常用的伺服器角色。 例如，Hyper-v 伺服器不需要圖形化使用者介面 (GUI) ，因為您可以從命令列使用 Windows PowerShell，或從遠端使用 Hyper-v 管理員來管理 Hyper-v 的所有層面。

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>不具 hyper-v 的 Server Core 差異-核心功能

當您在系統上完成安裝 Server Core 並第一次登入時，您會遇到一些驚喜。 伺服器與桌面體驗安裝選項和 Server Core 之間的主要差異在於 Server Core 不包含下列 GUI shell 封裝：

- Microsoft-Windows-伺服器-Shell-套件
- Microsoft-Windows-伺服器-Gui-管理套件
- Microsoft-Windows-伺服器-Gui-RSAT-套件
- Microsoft-Windows-Cortana-PAL-桌上型-套件

換句話說，Server Core 的設計並 **沒有桌面** 。 除了維護支援傳統商務應用程式和以角色為基礎的工作負載所需的功能之外，Server Core 還沒有傳統的桌面介面。 相反地，Server Core 的設計是透過命令列、PowerShell 或 GUI 工具（ (例如 [RSAT](../../remote/remote-server-administration-tools.md) 或 [Windows Admin Center](../../manage/windows-admin-center/overview.md)) ）從遠端系統管理。

除了沒有 UI 之外，Server Core 也會以下列方式與具有桌面體驗的伺服器不同：

- Server Core 沒有任何協助工具工具
- 沒有適用于設定 Server Core 的 OOBE (現成體驗) 
- 不支援音訊

下表顯示哪些應用程式可以在 Server Core vs Server 的 *本機* 上使用桌面體驗。 **重要** 事項：在大部分情況下，列為 [無法使用] 的應用程式可以從 Windows 用戶端電腦遠端執行，並用來管理 Server Core 安裝。

> [!NOTE]
> 這份清單適用于快速參考-它不是完整的清單。


| 應用程式                        | Server Core     | 具備桌面體驗的伺服器 |
|------------------------------------|-----------------|--------------------------------|
| 命令提示字元                     | 可供使用       | 可供使用                      |
| Windows PowerShell/Microsoft .NET | 可供使用       | 可供使用                      |
| Perfmon.exe                        | 無法使用   | 可供使用                      |
| Windbg (GUI)                        | 支援       | 支援                      |
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
| Wevtutil (事件查詢)            | 可供使用       | 可供使用                      |
| Services.msc                       | 無法使用   | 可供使用                      |
| 控制台                      | 無法使用   | 可供使用                      |
| Windows Update (GUI)                | 無法使用   | 可供使用                      |
| Windows 檔案總管                   | 無法使用   | 可供使用                      |
| 工作列                            | 無法使用   | 可供使用                      |
| 工作列通知              | 無法使用   | 可供使用                      |
| Taskmgr                            | 可供使用       | 可供使用                      |
| Internet Explorer 或邊緣          | 無法使用   | 可供使用                      |
| 內建說明系統               | 無法使用   | 可供使用                      |
| Windows 10 Shell                   | 無法使用   | 可供使用                      |
| Windows Media Player               | 無法使用   | 可供使用                      |
| PowerShell                         | 可供使用       | 可供使用                      |
| PowerShell ISE                     | 無法使用   | 可供使用                      |
| PowerShell IME                     | 可供使用       | 可供使用                      |
| Mstsc.exe                          | 無法使用   | 可供使用                      |
| 遠端桌面服務問題            | 可供使用       | 可供使用                      |
| Hyper-V 管理員                    | 無法使用   | 可供使用                      |
| 寫字 板\*                          | 無法使用   | 可供使用                      |


如需 Server *core 內含功能* 的詳細資訊，請參閱 [Windows server core 中包含的角色、角色服務和功能](server-core-roles-and-services.md)。 如需 Server Core *未* 包含之內容的相關資訊，請參閱 [不包含在 server Core 中的角色、角色服務和功能](server-core-removed-roles.md)。

\* 以讀取。RTF 檔案儲存在伺服器核心 SKU 上，使用者可以將檔案 (s) 複製到有 WordPad 的不同 Windows 電腦上。

## <a name="get-started-using-server-core"></a>開始使用 Server Core

您可以使用下列資訊來安裝、設定和管理 Windows Server 的 Server Core 安裝選項。

Server Core 安裝：
- [伺服器核心中包含的角色、角色服務和功能](server-core-roles-and-services.md)
- [不在 Server Core 中的角色、角色服務和功能](server-core-removed-roles.md)
- [安裝 Server Core 安裝選項](../../get-started/getting-started-with-server-core.md)
- [使用 SConfig 工具設定 Server Core](../../get-started/sconfig-on-ws2016.md)

使用 Server Core：
- [使用 Windows PowerShell 或命令列的基本 Server Core 管理工作](server-core-administer.md)
- [管理 Server Core](server-core-manage.md)
- [修補 Server Core](server-core-servicing.md)
- [設定記憶體傾印檔案](server-core-memory-dump.md)
