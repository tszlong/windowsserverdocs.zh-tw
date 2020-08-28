---
title: Set 選項
description: Set 選項的參考文章，設定建立陰影複製的選項。
ms.topic: reference
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0569e93ee5c732369b9bc07e4452d27558412b81
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036506"
---
# <a name="set-option"></a>Set 選項

設定用於建立陰影複製的選項。 如果使用時不含參數， **set 選項** 會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

### <a name="parameters"></a>參數

|     參數     |                                                                                                  描述                                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [差異   |                                                                                                     叢                                                                                                     |
|  以往容易  |                       指定陰影複製尚未匯入。 中繼資料 .cab 檔案稍後可以用來將陰影複製匯入相同或不同的電腦上。                       |
| [rollbackrecover] |                     通知寫入器在**PostSnapshot**事件期間使用*自動*回復。 如果陰影複製將用於回復 (例如，使用資料採礦) ，這會很有用。                      |
|   [txfrecover]    |                                                               要求 VSS 在建立期間以交易一致的方式進行陰影複製。                                                                |
|  [noautorecover]  | 停止寫入器和檔案系統，使陰影複製的任何復原變更都能以交易一致的狀態執行。 **Noautorecover** 不能搭配 **txfrecover** 或 **rollbackrecover**使用。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)