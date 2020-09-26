---
title: set 選項
description: Set option 命令的參考文章，設定建立陰影複製的選項。
ms.topic: reference
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a37e993700169f0268e68822be3a7fbaf6e58361
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389054"
---
# <a name="set-option"></a>set 選項

設定用於建立陰影複製的選項。 如果使用時不含參數， **set 選項** 會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| 差異 | 指定要建立指定磁片區的時間點快照集。 |
| 叢 | 指定在指定的磁片區上建立資料的時間點複製複本。 |
| 以往容易 | 指定陰影複製尚未匯入。 中繼資料 .cab 檔案稍後可以用來將陰影複製匯入相同或不同的電腦上。 |
| [rollbackrecover] | 通知寫入器在**PostSnapshot**事件期間使用*自動*回復。 如果陰影複製將用於回復 (例如，使用資料採礦) ，這會很有用。 |
| [txfrecover] | 要求 VSS 在建立期間以交易一致的方式進行陰影複製。 |
| [noautorecover] | 停止寫入器和檔案系統，使陰影複製的任何復原變更都能以交易一致的狀態執行。 **Noautorecover** 不能搭配 **txfrecover** 或 **rollbackrecover**使用。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [設定內容命令](set-context.md)

- [設定中繼資料命令](set-metadata.md)

- [設定詳細資訊命令](set-verbose.md)
