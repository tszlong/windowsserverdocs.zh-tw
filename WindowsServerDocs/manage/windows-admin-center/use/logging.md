---
title: 事件記錄
description: Windows 管理中心的事件記錄（Project 檀香山）
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 012c2229fb29aa711d9887f28859e09bcf71c14a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356865"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>使用 Windows 管理中心的事件記錄來深入瞭解管理活動，並追蹤閘道的使用方式

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows 系統管理中心會寫入事件記錄檔，讓您查看在您環境中的伺服器上執行的管理活動，以及協助您針對任何 Windows 系統管理中心問題進行疑難排解。

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>透過使用者動作記錄，深入瞭解環境中的管理活動

Windows 系統管理中心可讓您深入瞭解在環境中的伺服器上執行的管理活動，方法是將動作記錄到受管理伺服器的事件記錄檔中的**ServerManagementExperience**事件通道，並搭配 EventID 4000和來源 SMEGateway。 Windows 系統管理中心只會記錄受管理伺服器上的動作，因此如果使用者存取伺服器以進行唯讀，您就不會看到記錄的事件。

記錄的事件包括下列資訊：

| Key           | 值                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | 在伺服器上執行的 PowerShell 腳本名稱（如果動作已執行 PowerShell 腳本） |
| CIM           | 在伺服器上執行的 CIM 呼叫（如果動作已執行 CIM 呼叫）                        |
| 模組        | 執行動作的工具（或模組）                                                     |
| 閘道       | 執行動作的 Windows 管理中心閘道電腦名稱稱                     |
| UserOnGateway | 用來存取 Windows Admin Center 閘道並執行動作的使用者名稱                    |
| UserOnTarget  | 用來存取目標受管理伺服器的使用者名稱（如果不同于 userOnGateway）（亦即，使用者使用「管理身分」認證來存取伺服器） |
| 委派    | 布林值：如果目標受管理伺服器信任閘道，且認證是從使用者的用戶端電腦委派             |
| LAPS          | 布林值：如果使用者使用[LAPS](https://technet.microsoft.com/mt227395.aspx)認證存取伺服器                          |
| 檔案          | 已上傳檔案的名稱（如果動作是檔案上傳）                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>瞭解具有事件記錄的 Windows 管理中心活動

Windows 管理中心會將閘道活動記錄到閘道電腦上的事件通道，協助您針對問題進行疑難排解，並查看使用方式的計量。 這些事件會記錄到**Microsoft ServerManagementExperience**的事件通道。

[深入瞭解 Windows 管理中心的疑難排解。](troubleshooting.md)
