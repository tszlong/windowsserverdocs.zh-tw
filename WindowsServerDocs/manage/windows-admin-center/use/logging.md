---
title: 事件記錄
description: 從 Windows 系統管理中心 (Project Honolulu) 的事件記錄
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d91b92cb3bba99ae4aa96a96650a251a6df4cea5
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "2074339"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>使用 Windows 系統管理中心中記錄的事件可以深入了解管理活動和追蹤閘道流量

>適用於： Windows 系統管理中心，Windows 系統管理中心預覽

Windows 系統管理中心中寫入事件記錄檔以讓您查看您的環境中的伺服器上執行的管理活動，以及可協助您疑難排解任何 Windows 系統管理中心的問題。

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>掌握透過使用者動作記錄您環境中管理活動

Windows 系統管理中心提供在伺服器上執行您環境的事件記錄檔中的**Microsoft ServerManagementExperience**事件通道的受管理的伺服器，與 /eventid 4000 記錄動作的管理活動與來源 SMEGateway。 Windows 系統管理中心僅記錄的受管理的伺服器上的動作，讓您將不會看到如果使用者的唯讀存取伺服器記錄的事件。

記錄的事件包括下列資訊：

| 鍵           | 值                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | 如果巨集指令執行 PowerShell 指令碼的伺服器上，執行 PowerShell 指令碼名稱 |
| CIM           | 如果巨集指令執行 CIM 通話的伺服器上，執行 CIM 通話                        |
| 模組        | 工具 （或模組） 執行巨集指令                                                     |
| 閘道       | 執行巨集指令的 Windows 系統管理中心閘道機器名稱                     |
| UserOnGateway | 用來存取 Windows 系統管理中心閘道並執行巨集指令的使用者名稱                    |
| UserOnTarget  | 用來存取目標受管理的伺服器中，如果 userOnGateway （亦即使用使用"做為管理 」 的認證的伺服器存取的使用者） 與不同的使用者名稱 |
| 委派    | 布林值： 如果受管理的目標伺服器信任閘道並從使用者的用戶端電腦已委派認證             |
| 圈          | 布林值： 如果使用者存取使用[圈](https://technet.microsoft.com/mt227395.aspx)認證的伺服器                          |
| 檔案          | 上傳，如果動作是 「 檔案上傳的檔案名稱                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>了解使用事件記錄的 Windows 系統管理中心活動

Windows 系統管理中心記錄可協助您疑難排解問題及檢視使用狀況計量閘道電腦上的事件通道閘道活動。 **Microsoft ServerManagementExperience**事件通道會記錄下列事件。

[深入了解疑難排解 Windows 系統管理中心。](troubleshooting.md)
