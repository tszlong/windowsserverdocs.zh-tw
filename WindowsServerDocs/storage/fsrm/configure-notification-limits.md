---
title: 設定通知限制
description: 本文說明如何將時間限制新增至各種通知類型
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 625fb9c6a36717e57f3c6fd6da696b8df07fe8c3
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866057"
---
# <a name="configure-notification-limits"></a>設定通知限制

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

為了減少因為不斷超過配額閾值或嘗試儲存未授權檔案而累積的通知數量，檔案伺服器資源管理員會將時間限制套用至下列通知類型：

-   電子郵件
-   事件記錄檔
-   命令
-   報告

每項限制都會指定針對同樣問題產生另一相同類型已設定通知之前的一段時間。

每個通知類型預設會設定 60 分鐘數限制，但您可以變更這些限制。 限制會套用至指定類型的所有通知，不論這些通知是因為配額閾值還是檔案檢測事件而產生，都會套用。

## <a name="to-specify-a-standard-notification-limit-for-each-notification-type"></a>若要為每個通知類型指定標準通知限制

1.  在主控台樹狀目錄中，以滑鼠右鍵按一下 **\[檔案伺服器資源管理員\]**，然後按一下 **\[設定選項\]**。 [檔案伺服器資源管理員選項] 對話方塊隨即開啟。

2.  在 **\[通知限制\]** 索引標籤上，為每個顯示的通知類型輸入以分鐘為單位的值。

3.  按一下 [確定]。

> [!Note]
> 若要自訂與特定配額或檔案檢測之通知相關聯的時間限制，您可以使用檔案伺服器資源管理員命令列工具 **Dirquota.exe** 和 **Filescrn.exe**，或使用 [檔案伺服器資源管理員](/powershell/module/fileserverresourcemanager/) Cmdlet。

## <a name="additional-references"></a>其他參考資料

-   [設定檔案伺服器資源管理員選項](setting-file-server-resource-manager-options.md)
-   [命令列工具](command-line-tools.md)
