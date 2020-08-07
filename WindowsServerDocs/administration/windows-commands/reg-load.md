---
title: reg load
description: Reg load 命令的參考文章，其會將已儲存的子機碼和專案寫入登錄中的不同子機碼。
ms.topic: article
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc44a6d992312f67abb29a91da848cc17787d507
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884092"
---
# <a name="reg-load"></a>reg load

將已儲存的子機碼和專案寫入登錄中的不同子機碼。 此命令適用于用於疑難排解或編輯登錄專案的暫存檔案。

## <a name="syntax"></a>語法

```
reg load <keyname> <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname>` | 指定要載入之子機碼的完整路徑。 若要指定遠端電腦，請將電腦名稱稱的格式加上 `\\<computername>\`) 作為*keyname*的一部分 (。 省略 `\\<computername>\` 會使操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和**HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM**和**HKU**。 如果登錄機碼名稱包含空格，請用引號括住機碼名稱。  |
| `<filename>` | 指定要載入之檔案的名稱和路徑。 此檔案必須使用**reg save**命令事先建立，而且必須具有 hiv 副檔名。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Reg 載入**作業的傳回值如下：

    | 值 | 描述 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

### <a name="examples"></a>範例

若要將名為 TempHive. hiv 的檔案載入至金鑰 HKLM\TempHive，請輸入：

```
reg load HKLM\TempHive TempHive.hiv
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [reg save 命令](reg-save.md)
