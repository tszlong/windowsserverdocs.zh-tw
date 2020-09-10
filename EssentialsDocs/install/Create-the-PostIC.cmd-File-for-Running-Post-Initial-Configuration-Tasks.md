---
title: 建立 PostIC.cmd 檔案以便執行初始設定後續的工作
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 99e258bc-0695-48c9-b694-a7f3cbe2a2d0
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 5a3ae6711d54d1c1f2bebdae6db3065fce970312
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623717"
---
# <a name="create-the-posticcmd-file-for-running-post-initial-configuration-tasks"></a>建立 PostIC.cmd 檔案以便執行初始設定後續的工作

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

您可以撰寫自己的程式碼，然後從名為 PostIC.cmd 的指令碼檔案呼叫該程式碼，藉此來新增後置初始設定的自訂項目。 使用 PostIC.cmd 檔案時，必須遵守下列規定：

- 您的自訂程式碼必須以無訊息模式執行 (即不能顯示使用者介面)。

- 您的自訂程式碼不能起始伺服器的重新啟動。 初始設定會將重新啟動伺服器作為最後一個工作。

- 您的自訂程式碼必須在三分鐘以內執行完成。

  定義您的 PostIC.cmd 檔案，使其在程式碼執行成功時傳回 0。 如果傳回其他值，作業系統就會尋找名為 [SetupFailure.cmd](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md#BKMK_SetupFailure) 的檔案，這個檔案包含當 PostIC.cmd 檔案中的程式碼執行失敗時，應該執行的程式碼。 PostIC.cmd 檔案和 SetupFailure.cmd 檔案都必須位於 C:\Windows\Setup\Scripts 中。

#### <a name="to-define-post-initial-configuration-customizations"></a>若要定義初始設定的後續自訂項目

1.  撰寫從 PostIC.cmd 指令碼呼叫的程式碼。

2.  使用記事本建立名為 PostIC.cmd 的檔案，然後新增對您在步驟 1 所建立程式碼的呼叫。 確保您的程式碼傳回成功值。

3.  將 PostIC.cmd 儲存在 C:\Windows\Setup\Scripts 中。

4.  (選用) 建立 SetupFailure.cmd 檔案，使其在 PostIC.cmd 傳回非 0 的值時執行程式碼。

###  <a name="setupfailurecmd"></a><a name="BKMK_SetupFailure"></a> Setupfailure.cmd .cmd
 您可以使用 SetupFailure.cmd，在初始設定中提供問題通知。 SetupFailure.cmd 檔案包含您要在發生問題時執行的程式碼。 SetupFailure.cmd 檔案會放置於 C:\Windows\Setup\Scripts 中，且會在設定工作發生問題或是 PostIC.cmd 檔案傳回非 0 的值時執行。

##### <a name="to-define-notifications"></a>若要定義通知

1.  撰寫從 SetupFailure.cmd 指令碼呼叫的程式碼。

2.  使用記事本建立名為 SetupFailure.cmd 的檔案，然後新增對您在步驟 1 所建立程式碼的呼叫。 確保您的程式碼傳回成功值。

3.  在 C:\Windows\Setup\Scripts 中儲存 SetupFailure.cmd。

## <a name="see-also"></a>另請參閱
 [使用 Windows Server ESSENTIALS ADK 消費者入門](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)[建立和自訂映射](Creating-and-Customizing-the-Image.md)[其他自訂](Additional-Customizations.md)專案[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)