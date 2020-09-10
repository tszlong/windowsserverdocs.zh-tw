---
title: reg import
description: Reg import 命令的參考文章，此命令會將包含已匯出之登錄子機碼、專案和值的檔案內容複寫到本機電腦的登錄中。
ms.topic: reference
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 49f83a20b4f9c9e1e64f6e33f0980757de60d9ec
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627038"
---
# <a name="reg-import"></a>reg import

將包含已匯出之登錄子機碼、專案和值的檔案內容複寫到本機電腦的登錄中。

## <a name="syntax"></a>語法

```
reg import <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<filename>` | 指定檔案的名稱和路徑，該檔案的內容要複製到本機電腦的登錄中。 您必須使用 **reg export**事先建立此檔案。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Reg 匯入**作業的傳回值為：

    | 值 | 描述 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

### <a name="examples"></a>範例

若要從名為 AppBkUp 的檔案匯入登錄專案，請輸入：

```
reg import AppBkUp.reg
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [reg export 命令](reg-export.md)
