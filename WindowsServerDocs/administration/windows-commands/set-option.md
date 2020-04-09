---
title: Set 選項
description: 設定選項的 Windows 命令主題，可設定陰影複製的建立選項。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a956c74619f3a6c33dfcaa62ee4c810cd93b21f1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834491"
---
# <a name="set-option"></a>Set 選項

設定陰影複製的建立選項。 如果使用時不含參數， **set option**會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

### <a name="parameters"></a>參數

|     參數     |                                                                                                  描述                                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [差異   |                                                                                                     複雜                                                                                                     |
|  以往容易  |                       指定陰影複製尚未匯入。 中繼資料 .cab 檔案稍後可以用來將陰影複製匯入相同或不同的電腦。                       |
| [rollbackrecover] |                     通知寫入器在**PostSnapshot**事件期間使用「*自動*回復」。 如果陰影複製將用於復原（例如，使用資料採礦），這會很有用。                      |
|   [txfrecover]    |                                                               要求 VSS 在建立期間讓陰影複製保持一致。                                                                |
|  [noautorecover]  | 停止寫入器和檔案系統，將陰影複製的任何復原變更執行到交易一致的狀態。 **Noautorecover**不能與**txfrecover**或**rollbackrecover**搭配使用。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)