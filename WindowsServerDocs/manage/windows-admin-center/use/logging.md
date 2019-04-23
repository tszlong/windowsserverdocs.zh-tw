---
title: 事件記錄
description: 從 Windows Admin Center （專案檀香山） 的事件記錄
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d91b92cb3bba99ae4aa96a96650a251a6df4cea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849299"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>若要深入了解管理活動和追蹤閘道的使用方式使用 Windows Admin Center 的事件記錄

>適用於：Windows Admin Center，Windows Admin Center 預覽

Windows Admin Center 寫入事件記錄檔，讓您查看您的環境中的伺服器上執行管理活動，以及可協助您疑難排解 Windows Admin Center 中的任何問題。

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>深入了解您的環境，透過使用者的動作記錄中的管理活動

Windows Admin Center 提供您環境中的伺服器上所記錄的動作，以執行管理活動的深入**Microsoft ServerManagementExperience**受管理的事件記錄檔中的事件通道EventID 4000 與來源 SMEGateway 的伺服器。 Windows Admin Center 只會記錄動作上的受管理的伺服器，因此您不會看到如果使用者的唯讀存取的伺服器所記錄的事件。

記錄的事件包含下列資訊：

| Key           | 值                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | 如果動作已執行的 PowerShell 指令碼的伺服器，執行的 PowerShell 指令碼名稱 |
| CIM           | 如果動作已執行的 CIM 呼叫的伺服器，執行的 CIM 呼叫                        |
| 模組        | 工具 （或模組） 已在其中執行的動作                                                     |
| 閘道       | 動作已執行的 Windows Admin Center 閘道電腦的名稱                     |
| UserOnGateway | 用來存取 Windows Admin Center 閘道，並執行動作的使用者名稱                    |
| UserOnTarget  | 如果不同於 userOnGateway （也就是使用者存取伺服器並使用 管理者身分 」 認證），用來存取目標受管理的伺服器的使用者名稱 |
| 委派    | 布林值： 如果目標管理伺服器信任閘道並從使用者的用戶端電腦所委派的認證             |
| LAPS          | 布林值： 如果伺服器使用的使用者存取[LAPS](https://technet.microsoft.com/mt227395.aspx)認證                          |
| 檔案          | 上傳，如果動作是檔案上傳的檔案名稱                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>了解與事件記錄的 Windows Admin Center 活動

Windows Admin Center 會將閘道活動記錄到事件通道，可協助您疑難排解問題，以及檢視使用量計量在閘道電腦上。 這些事件會記錄到**Microsoft ServerManagementExperience**事件通道。

[深入了解疑難排解 Windows Admin Center。](troubleshooting.md)
