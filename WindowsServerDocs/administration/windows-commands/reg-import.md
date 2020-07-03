---
title: reg import
description: Reg import 命令的參考文章，它會將包含所匯出登錄子機碼、專案和值的檔案內容複寫到本機電腦的登錄中。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 77c8284dd2341f37292afdfd810b2182686aad68
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931862"
---
# <a name="reg-import"></a>reg import

將包含已匯出登錄子機碼、專案和值的檔案內容複寫到本機電腦的登錄中。

## <a name="syntax"></a>語法

```
reg import <filename>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `<filename>` | 指定檔案的名稱和路徑，該檔案包含要複製到本機電腦之登錄中的內容。 必須事先使用**reg export**建立此檔案。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Reg 匯入**作業的傳回值如下：

    | 值 | 說明 |
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
