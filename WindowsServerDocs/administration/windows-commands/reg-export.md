---
title: reg export
description: Reg export 命令的參考文章，它會將本機電腦的指定子機碼、專案和值複製到檔案中，以傳輸到其他伺服器。
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 31f59aca51b74150682a5ba3085b7ffcef058d29
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884135"
---
# <a name="reg-export"></a>reg export

將本機電腦的指定子機碼、專案和值複製到檔案中，以傳輸到其他伺服器。

## <a name="syntax"></a>語法

```
reg export <keyname> <filename> [/y]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname>` | 指定子機碼的完整路徑。 匯出操作僅適用于本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和**HKCC**。 如果登錄機碼名稱包含空格，請用引號括住機碼名稱。 |
| `<filename>` | 指定要在作業期間建立之檔案的名稱和路徑。 檔案的副檔名必須是 .reg。 |
| /y | 以名稱*filename*覆寫任何現有的檔案，而不提示確認。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Reg 匯出**作業的傳回值如下：

    | 值 | 描述 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

### <a name="examples"></a>範例

若要將金鑰 MyApp 的所有子機碼和值匯出至 AppBkUp 檔案，請輸入：

```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
